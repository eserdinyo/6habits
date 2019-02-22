<template lang='pug'>
 .main(@click='closeEdit')
  h1.title 6 HABITS
  div(:class='{ overlay: showOverlay }')
  .container.wrapper
    .habit.col-md-4(v-for='(habit,index) in habits', 
      :key='habit.id', :class='{ done: habit.status, onDeletingHabit: deletingHabit }', 
      @click.stop="done('http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3',index)")
      i(:class="['habit__icon fas fa-' + habit.icon]")
      p.habit__title {{ habit.name}}
      .habit__count
        p {{habit.count}}
        i.fas.fa-star
      a.habit__delete.fas.fa-times(@click='deleteHabit', v-if='deletingHabit')
    .habit.habit__add.col-md-4(@click.stop="openModal",v-if='deletingHabit')
      i(:class="['habit__icon fas fa-plus']")
      p.habit__title Add Habit
    Setting
    .setting-btn(@click='toggleSetting')
      i.fas.fa-cog
    .edit-btn(@click.stop='activeEdit')
      i.fas.fa-pen
    Modal
  
</template>

<script>
import Setting from "./Setting";
import Modal from './AddHabitModal';
import { EventBus } from "../main";

export default {
  name: "Home",
  components: {
    Setting,
    Modal
  },
  data() {
    return {
      interval: false,
      count: 0,
      deletingHabit: false,
      showSetting: false,
      HABITS_KEY: "habits",
      showOverlay: false,
      habits: "",
      habitsDummy: [
        {
          name: "Gitar Çalış",
          icon: "music",
          count: 1,
          status: false
        },
        {
          name: "Fotoğraf Çek",
          icon: "camera",
          count: 1,
          status: false
        },
        {
          name: "5 Bardak Su İç",
          icon: "tint",
          count: 1,
          status: false
        },
        {
          name: "30 dk Yürüyüş",
          icon: "walking",
          count: 1,
          status: false
        },
        {
          name: "10 dk Kitap Oku",
          icon: "book",
          count: 1,
          status: false
        }
      ]
    };
  },
  methods: {
    deleteHabit() {
      console.log("delete...");
    },
    closeEdit() {
      this.deletingHabit = false;
    },
    done(sound, habit) {
      if (!this.deletingHabit) {
        if (!this.habits[habit].status) {
          this.habits[habit].status = true;
          this.habits[habit].count += 1;

          var audio = new Audio(sound);
          audio.play();
        }
      }
    },
    toggleSetting() {
      EventBus.$emit("openSetting", 2);
      this.showOverlay = true;
    },

    saveToStorage() {
      localStorage.setItem(this.HABITS_KEY, JSON.stringify(this.habitsDummy));
    },
    fetchFromStorage() {
      this.habits = JSON.parse(localStorage.getItem(this.HABITS_KEY));
      this.habits.forEach((habit, index) => {
        habit.id = index;
        habit.status = false;
      });
    },
    activeEdit() {
      this.deletingHabit = true;
      console.log("edit...");
    },
    openModal() {}
  },
  created() {
    this.fetchFromStorage();
    EventBus.$on("closeOverlay", payload => {
      this.showOverlay = false;
    });
  },
  watch: {
    habits: {
      handler(habits) {
        this.saveToStorage(this.habits);
      },
      deep: true
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 20px;
  padding-right: 20px;
}

.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 4rem;
  justify-items: center;
  text-align: center;
}

.title {
  color: #fff;
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
  font-size: 3rem;
  text-transform: uppercase;
  margin-bottom: 5%;
}

.habit {
  width: 150px;
  height: 150px;
  margin-bottom: 7%;
  border: 7px solid #191919;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  cursor: pointer;
  transform-origin: top right;

  &__icon {
    font-size: 4rem;
    color: #fff;
    position: absolute;
  }

  &__title {
    font-size: 1rem;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: bold;
    text-transform: uppercase;
    position: absolute;
    top: 100%;
    color: #fff;
    margin-top: 20px;
  }

  &__count {
    position: absolute;
    top: 70%;
    display: flex;
    align-items: center;

    & .fas {
      margin-left: 3px;
    }
  }

  .fa-star {
    color: #fff;
    font-size: 10px;
  }

  &__delete {
    position: absolute;
    right: 0;
    top: -5%;
    color: #fff;
    font-size: 20px;
  }

  &__add {
    background-color: #fff;
    border-color: #fff;

    .habit__icon {
      color: #6ab04c;
    }
  }
}

.done {
  background-color: #ecf0f1;
  color: #191919;
  border-color: #ecf0f1;

  & .habit__icon {
    color: #191919;
  }

  & .habit__count {
    & .fas {
      color: #191919;
    }
  }
}

.setting-btn {
  position: absolute;
  left: 20px;
  bottom: 20px;
  transition: all 0.2s;
  cursor: pointer;
  z-index: 9;

  &:hover {
    transform: rotate(30deg);
  }
}

.edit-btn {
  position: absolute;
  right: 20px;
  bottom: 20px;
  transition: all 0.2s;
  cursor: pointer;
  z-index: 9;
  color: #fff;
}

.overlay {
  height: 100vh;
  width: 100vw;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 99;
  background-color: #191919bb;
}

.onDeletingHabit {
  animation: jiggle 0.2s infinite;
}

@keyframes jiggle {
  0% {
    transform: rotate(-1deg);
  }
  50% {
    transform: rotate(1deg);
  }
}
</style>
