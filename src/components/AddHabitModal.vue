<template lang='pug'>
    .Modal
      .Modal__close.fas.fa-times(@click.stop='closeModal')
      .Modal__title Add Habit
      input.Modal__habit-name(v-model='habit.habitName', placeholder='Habit name').Modal__name
      .Modal__icons
        a(v-for='icon in icons', 
          :class='{selectedIcon:icon == iconName}',
          @click='selectIcon(icon)')
           i(:class="['fas fa-' + icon]")
      a.Modal__save(@click='addHabit') Save
</template>

<script>
import { EventBus } from "../main";

export default {
  data() {
    return {
      habit: {
        habitName: "",
        icon: ""
      },
      iconName: "",
      selectedIcon: true,
      icons: [
        "music",
        "camera",
        "tint",
        "walking",
        "book",
        "baseball-ball",
        "bed",
        "beer",
        "bus",
        "heart",
        "fire",
        "calculator",
        "car",
        "carrot",
        "music",
        "camera",
        "tint",
        "baby",
        "book",
        "dumbbell",
        "bicycle",
        "crow",
        "cat",
        "dog"
      ]
    };
  },
  methods: {
    selectIcon(iconName) {
      this.iconName = iconName;
      this.habit.icon = iconName;
    },
    addHabit() {
      let value = this.habit.habitName.trim() && this.habit.icon;
      if (!value) return;
      EventBus.$emit("addHabit", this.habit);
      this.habit.habitName = "";
      this.habit.icon = "";
      EventBus.$emit("closeModal", 1);
    },
    closeModal() {
      EventBus.$emit("closeModal", 1);
    }
  }
};
</script>

<style lang='scss' scoped>
.Modal {
  background-color: #ddd;
  padding: 40px 100px;
  border-radius: 5px;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 99;

  &__title {
    font-size: 20px;
    font-weight: bold;
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  &__habit-name {
    padding: 5px;
    border: 1px solid #eee;
    outline: none;
    transition: all 0.2s;

    &:focus {
      border-bottom-color: #6ab04c;
    }
  }

  &__icons {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 10px;
    height: 100px;
    overflow-y: scroll;
    margin-top: 20px;
    background-color: #fff;
    margin-bottom: 20px;
    padding: 10px;

    a {
      cursor: pointer;
      padding: 3px;
      color: #333;
      border-radius: 2px;
    }
  }

  &__save {
    background-color: #27ae60;
    border-radius: 3px;
    text-transform: uppercase;
    cursor: pointer;
    color: #fff;
    font-size: 14px;
    padding: 5px 20px;
  }

  &__close {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 20px;
    cursor: pointer;
  }
}

.selectedIcon {
  background-color: #27ae60;
  color: #fff !important;
}
</style>