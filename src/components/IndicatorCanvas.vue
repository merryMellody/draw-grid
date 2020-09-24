<template>
  <canvas
    id="indicatorCanvas"
    v-bind:width="appWidth + 'px'"
    v-bind:height="appHeight + 'px'"
  ></canvas>
</template>

<script>
export default {
  name: "IndicatorCanvas",
  props: {
    appWidth: {
      type: Number,
      default: 640,
    },
    appHeight: {
      type: Number,
      default: 480,
    },
    gridSize: {
      type: Number,
      default: 80,
    }
  },
  data() {
    return {
      indicatorCtx: null,
    };
  },
  mounted() {
    const indicatorCanvas = document.querySelector("#indicatorCanvas");
    this.indicatorCtx = indicatorCanvas.getContext("2d");
  },
};
</script>

<style>
#indicatorCanvas {
  position: absolute;
  border: 5px solid black;
  border-radius: 1%;
  z-index: 20;
  cursor: crosshair;
}
</style>