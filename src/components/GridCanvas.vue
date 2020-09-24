<template>
  <canvas
    id="gridCanvas"
    ref="gridCanvas"
    v-bind:width="appWidth + 'px'"
    v-bind:height="appHeight + 'px'"
  ></canvas>
</template>

<script>
export default {
  name: "GridCanvas",
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
    },
  },
  data() {
    return {
      gridCtx: null,
    };
  },
  methods: {
    drawGrid() {
      if (this.gridCtx) {
        this.gridCtx.clearRect(0, 0, this.appWidth, this.appHeight);

        this.gridCtx.lineWidth = 1.5;

        for (let x = this.gridSize; x < this.appWidth; x += this.gridSize) {
          this.gridCtx.beginPath();
          this.gridCtx.moveTo(x, 0);
          this.gridCtx.lineTo(x, this.appHeight);
          this.gridCtx.stroke();
        }

        for (let y = this.gridSize; y < this.appHeight; y += this.gridSize) {
          this.gridCtx.beginPath();
          this.gridCtx.moveTo(0, y);
          this.gridCtx.lineTo(this.appWidth, y);
          this.gridCtx.stroke();
        }
      }
    },
  },
  watch: {
    appHeight: function () {
      this.$nextTick(() => {
        this.drawGrid();
      });
    },
    appWidth: function () {
      this.$nextTick(() => {
        this.drawGrid();
      });
    },
    gridSize: function () {
      this.$nextTick(() => {
        this.drawGrid();
      });
    },
  },
  mounted() {
    const gridCanvas = this.$refs.gridCanvas;
    this.gridCtx = gridCanvas.getContext("2d");

    this.$nextTick(() => {
      this.drawGrid();
    });
  },
};
</script>

<style>
#gridCanvas {
  position: absolute;
  z-index: 10;
  cursor: crosshair;
}
</style>