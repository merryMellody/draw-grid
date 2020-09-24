<template>
  <div id="app">
    <div id="canvasLayers">
      <canvas
        id="backgroundCanvas"
        v-bind:width="appWidth + 'px'"
        v-bind:height="appHeight + 'px'"
      ></canvas>
      <GridCanvas
        :appHeight="appHeight"
        :appWidth="appWidth"
        :gridSize="gridSize"
      />
      <IndicatorCanvas
        :appHeight="appHeight"
        :appWidth="appWidth"
        :gridSize="gridSize"
      />
    </div>
    <div id="toggles" v-bind:style="{ top: togglesTop + 'px', left: '13px' }">
      <span id="hello">{{ message }}</span>
      <input
        type="number"
        v-model.number="appHeight"
        placeholder="App Height"
      />
      <input type="number" v-model.number="appWidth" placeholder="App Width" />
      <input type="number" v-model.number="gridSize" placeholder="Grid Size" />
    </div>
  </div>
</template>

<script>
import GridCanvas from "./components/GridCanvas.vue";
import IndicatorCanvas from "./components/IndicatorCanvas.vue";

export default {
  name: "App",
  components: {
    GridCanvas,
    IndicatorCanvas,
  },
  data() {
    return {
      message: "Hello Vue!",
      appWidth: 640,
      appHeight: 480,
      gridSize: 80,
    };
  },
  computed: {
    togglesTop() {
      return Number(this.appHeight) + 18;
    },
  },
};
</script>

<style>
body {
  background-color: black;
  -webkit-user-select: none; /* Safari */
  -ms-user-select: none; /* IE 10+ and Edge */
  user-select: none; /* Standard syntax */
}

#canvasLayers {
  position: relative;
}

#backgroundCanvas {
  position: absolute;
  background-color: white;
  z-index: 0;
}

#toggles {
  position: absolute;
}

#hello {
  color: white;
  position: relative;
}
</style>
