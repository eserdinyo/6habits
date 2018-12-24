<template>
  <div class="main">
    <h1 class="title">6 HABITS</h1>
    <div :class="{ overlay: showSetting }"></div>
    <div class="container wrapper">
      <div
        v-for="(habit,index) in habits"
        :key="habit.id"
        class="habit col-md-4"
        :class="{ done: habit.status }"
        @click="done('http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3',index)"
      >
        <i :class="['habit__icon fas fa-' + habit.icon]"></i>
        <p class="habit__title">{{ habit.name}}</p>
        <div class="habit__count">
          <p>{{habit.count}}</p>
          <i class="fas fa-star"></i>
        </div>
      </div>

      <div class="setting" :class="{ showSetting: showSetting }">
        <div @click="closeSetting" class="close-btn">
          <i class="fas fa-times"></i>
        </div>

        <div class="colors-wrapper">
          <h3>Background Color</h3>

          <div class="colors">
            <div
              class="color"
              v-for="color in colors"
              @click="saveBackgroundColor(color)"
              :style="{backgroundColor: color}"
              :key="color.id"
            ></div>
          </div>
        </div>
      </div>
      <div @click="toggleSetting" class="setting-btn">
        <i class="fas fa-cog"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      interval: false,
      count: 0,
      showSetting: false,
      HABITS_KEY: "habits",
      BACKGROUND_KEY: "background",
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
        },
        {
          name: "10 dl Egzersiz Yap",
          icon: "dumbbell",
          count: 4,
          status: false
        }
      ],
      colors: [
        "#4834d4",
        "#F97F51",
        "#be2edd",
        "#eb4d4b",
        "#f9ca24",
        "#6ab04c",
        "#0984e3",
        "#2d3436",
        "#6D214F"
      ]
    };
  },
  methods: {
    done(sound, habit) {
      if (!this.habits[habit].status) {
        this.habits[habit].status = true;
        this.habits[habit].count += 1;

        var audio = new Audio(sound);
        audio.play();
      }
    },
    toggleSetting() {
      this.showSetting = !this.showSetting;
    },
    closeSetting() {
      this.showSetting = false;
    },
    saveBackgroundColor(color) {
      localStorage.setItem(this.BACKGROUND_KEY, color);
      document.body.style.backgroundColor = color;
    },
    saveToStorage() {
      localStorage.setItem(this.HABITS_KEY, JSON.stringify(this.habits));
    },
    fetchFromStorage() {
      this.habits = JSON.parse(localStorage.getItem(this.HABITS_KEY));
      this.habits.forEach((habit, index) => {
        habit.id = index;
        habit.status = false;
      });
    }
  },
  created() {
    this.fetchFromStorage();
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

  &:hover {
    //border-color: #ecf0f1;
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

.setting {
  height: 100vh;
  background: #22a6b3;
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  opacity: 0;
  transition: all 0.2s;
  z-index: 9999;
}

.showSetting {
  width: 40vw;
  opacity: 1;
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

.close-btn {
  position: absolute;
  top: 20px;
  left: 20px;
  color: #fff;
  font-size: 20px;
  cursor: pointer;
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

.colors-wrapper {
  position: absolute;
  top: 100px;
  left: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;

  h3 {
    text-align: center;
    color: #fff;
  }
}
.colors {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  grid-gap: 40px;

  .color {
    height: 20px;
    width: 20px;
    background: yellow;
    border-radius: 50%;
    cursor: pointer;
  }
}
</style>