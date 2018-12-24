<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script>
export default {
  data() {
    return {
      backgroundColor: "",
    }
  },

  created() {
      this.backgroundColor = localStorage.getItem("background");
      document.body.style.backgroundColor = this.backgroundColor;
  }
}
</script>

<style lang="scss">
body {
  font-family: 'Roboto', sans-serif;
}
</style>
