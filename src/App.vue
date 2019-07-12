<template>
  <div id="app" :style="{backgroundColor}" @click="closeEdit">
    <Home />
  </div>
</template>
<script>
import Home from "@/components/Home";
import { EventBus } from "@/main";

export default {
  data() {
    return {
      backgroundColor: ""
    };
  },
  components: {
    Home
  },
  methods: {
    closeEdit() {
      EventBus.$emit('closeEdit', 1);
    }
  },

  created() {
    if (!localStorage.getItem("background")) {
      this.backgroundColor = "#8e44ad";
    } else this.backgroundColor = localStorage.getItem("background");

    EventBus.$on("changeColor", color => {
      this.backgroundColor = color;
    });
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  font-family: "Roboto", sans-serif;
}
#app {
  height: 100vh;
}
</style>
