<template>
  <div class="main">
    <h1 class="title">6 HABITS</h1>
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
          <p>1</p>
          <i class="fas fa-star"></i>
        </div>
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
      habits: [
        {
          name: "Gitar Çalış",
          icon: "music",
          status: false
        },
        {
          name: "Fotoğraf Çek",
          icon: "camera",
          status: false
        },
        {
          name: "5 Bardak Su İç",
          icon: "tint",
          status: false
        },
        {
          name: "30 dk Yürüyüş",
          icon: "walking",
          status: false
        },
        {
          name: "10 dk Kitap Oku",
          icon: "book",
          status: false
        },
        {
          name: "10 dl Egzersiz Yap",
          icon: "dumbbell",
          status: false
        }
      ]
    };
  },
  methods: {
    done(sound,habit) {
      console.log(habit);
      this.habits[habit].status = true;
      
      var audio = new Audio(sound);
      audio.play();
    
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
</style>