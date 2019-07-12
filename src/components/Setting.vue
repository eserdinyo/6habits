<template lang='pug'>
  .setting(:class='{ showSetting: showSetting }')
    .close-btn(@click='closeSetting')
        i.fas.fa-times
    .colors-wrapper
        h3 Background Color
        .colors
            .color(v-for='color in colors', 
                    @click.stop='saveBackgroundColor(color)', 
                    :style='{backgroundColor: color}', 
                    :key='color.id')
</template>

<script>
import { EventBus } from "../main";

export default {
  data() {
    return {
      BACKGROUND_KEY: "background",
      showSetting: false,
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
    saveBackgroundColor(color) {
      localStorage.setItem(this.BACKGROUND_KEY, color);
      EventBus.$emit("changeColor", color);
    },
    closeSetting() {
      this.showSetting = false;
      EventBus.$emit("closeOverlay", 2);
    }
  },
  created() {
    EventBus.$on("openSetting", payload => {
      payload == 1 ? (this.showSetting = true) : this.closeSetting();
    });
  }
};
</script>

<style lang='scss'>
.setting {
  height: 100vh;
  background: #22a6b3;
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  transition: all 0.2s;
  z-index: 9999;
  display: none;
}

.showSetting {
  width: 20vw;
  display: unset;
}

.close-btn {
  position: absolute;
  top: 20px;
  left: 20px;
  color: #fff;
  font-size: 20px;
  cursor: pointer;
}

.colors-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
  margin-top: 40px;

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