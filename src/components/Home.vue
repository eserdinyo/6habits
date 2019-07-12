<template lang='pug'>
 div
  div.title 6 HABITS
  div(:class='{ overlay: showOverlay }')
  .container.wrapper
    .habit__create(v-if='habits.length == 0')
      h3.habit__create--title Create a habit
      i.habit__create--arrow.fa.fa-arrow-up
    .habit.col-md-4(v-for='(habit,index) in habits',
      :key='habit.id', :class='{ done: habit.status, onDeletingHabit: deletingHabit }',
      @click.stop="done('http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3',index)")
      i(:class="['habit__icon fas fa-' + habit.icon]")
      p.habit__title {{ habit.name}}
      .habit__count
        p {{habit.count}}
        i.fas.fa-star
      a.habit__delete.fas.fa-times(@click='removeHabit(habit)', v-if='deletingHabit')
    .habit.habit__add.col-md-4(@click.stop="openModal",
        v-if='deletingHabit && habits.length < 6')
      i(:class="['habit__icon fas fa-plus']")
      p.habit__title Add Habit
    Setting
    .setting-btn(@click.stop='toggleSetting')
      i.fas.fa-cog
    .edit-btn(@click.stop='activeEdit')
      i.fas.fa-pen
    Modal(v-if='isModalOpen')

</template>

<script>
import Setting from "./Setting";
import Modal from "./AddHabitModal";
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
      isModalOpen: false,
      showSetting: false,
      HABITS_KEY: "habits",
      showOverlay: false,
      habits: [],
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
    removeHabit(habit) {
      this.habits.splice(this.habits.indexOf(habit), 1);
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
      EventBus.$emit("openSetting", 1);
      this.showOverlay = true;
    },

    saveToStorage(habits) {
      localStorage.setItem(this.HABITS_KEY, JSON.stringify(habits));
    },
    fetchFromStorage() {
      if (JSON.parse(localStorage.getItem(this.HABITS_KEY))) {
        this.habits = JSON.parse(localStorage.getItem(this.HABITS_KEY));
        this.habits.forEach((habit, index) => {
          // habit.status = false;
        });
      }
    },
    activeEdit() {
      this.deletingHabit = true;
    },
    openModal() {
      this.isModalOpen = true;
      this.showOverlay = true;
    },
    getTime() {
      const time = new Date();
      const getHour = time.getHours();
      const getMin = time.getMinutes();
      const getSec = time.getSeconds();

      if (getHour == 14) {
        if (JSON.parse(localStorage.getItem(this.HABITS_KEY))) {
          this.habits = JSON.parse(localStorage.getItem(this.HABITS_KEY));
        }
        this.habits.forEach((habit, index) => {
          if (habit.status == false) {
            habit.count = 0;
          }
        });
      }
    },
    hasOneDayPassed() {
      const date = new Date().toLocaleDateString();
      if (localStorage.getItem("date") == date) return false;
      else {
        // Sıfırlama işletmi
        this.habits.forEach((habit, index) => {
          if (habit.status == false) habit.count = 0;
          habit.status = false;
        });
        localStorage.setItem("date", date);
      }
    }
  },
  created() {
    this.fetchFromStorage();
    this.getTime();
    this.hasOneDayPassed();

    EventBus.$on("closeOverlay", payload => {
      this.showOverlay = false;
    });

    EventBus.$on("closeModal", payload => {
      this.showOverlay = false;
      this.isModalOpen = false;
    });

    EventBus.$on("addHabit", habit => {
      this.habits.push({
        name: habit.habitName,
        icon: habit.icon,
        count: 0,
        status: false
      });
    });

    EventBus.$on("closeEdit", payload => {
      if (!this.isModalOpen) {
        this.deletingHabit = false;
        EventBus.$emit("openSetting", 0);
      }
    });
  },
  watch: {
    habits: {
      // Works when habits array change
      handler(habits) {
        this.saveToStorage(habits);
      },
      deep: true
    }
  }
};
</script>

<style lang="scss">
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
  margin-bottom: 40px;
  text-transform: uppercase;
  padding-top: 30px;
}

.habit {
  width: 150px;
  height: 150px;
  margin-bottom: 7%;
  border: 7px solid #ecf0f1;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  cursor: pointer;
  transform-origin: top right;

  &__create {
    color: #fff;
    text-transform: uppercase;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    &--arrow {
      position: absolute;
      right: -50%;
      font-size: 50px;
      transform: rotate(135deg);
    }
  }

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
    color: #fff;

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
  color: #333;

  & .habit__icon {
    color: #333;
  }

  & .habit__count {
    & .fas {
      color: #333;
    }
  }

  & .habit__count p {
    color: #333;
  }
}

.setting-btn {
  position: absolute;
  left: 20px;
  bottom: 20px;
  transition: all 0.2s;
  cursor: pointer;
  z-index: 9;
  color: #ddd;

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
  color: #ddd;
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
