<template>
  <canvas
    id="indicatorCanvas"
    ref="indicatorCanvas"
    v-bind:width="appWidth + 'px'"
    v-bind:height="appHeight + 'px'"
    @touchmove="handleTouchMove"
    @touchstart="handleTouchStart"
    @touchend="handleTouchEnd"
    @mousemove="handleMouseMove"
    @mousedown="handleMouseDown"
    @mouseup="handleMouseUp"
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
    },
  },
  data() {
    return {
      indicatorCtx: null,
      gridPos: {
        x: -1,
        y: -1,
      },
      gridFirstPos: {
        x: -1,
        y: -1,
      },
      mouseDown: false,
      touchDown: false,
    };
  },
  methods: {
    handleMouseMove(e) {
      const relativeX = e.clientX - this.$refs.indicatorCanvas.offsetLeft - 9;
      const relativeY = e.clientY - this.$refs.indicatorCanvas.offsetTop - 9;

      const { x: gridX, y: gridY } = this.retrieveGridCoord(
        relativeX,
        relativeY
      );

      this.gridPos = {
        x: gridX,
        y: gridY,
      };

      this.$nextTick(() => {
        this.drawIndicators();
      });
    },
    handleMouseDown() {
      this.mouseDown = true;

      this.gridFirstPos = {
        x: this.gridPos.x,
        y: this.gridPos.y,
      };

      this.$nextTick(() => {
        this.drawIndicators();
      });
    },
    handleMouseUp() {
      this.mouseDown = false;

      this.$nextTick(() => {
        this.drawIndicators();
      });
    },
    handleTouchMove(e) {
      e.preventDefault();

      const relativeX =
        e.touches[0].pageX - this.$refs.indicatorCanvas.offsetLeft - 9;
      const relativeY =
        e.touches[0].pageY - this.$refs.indicatorCanvas.offsetLeft - 9;

      const { x: gridX, y: gridY } = this.retrieveGridCoord(
        relativeX,
        relativeY
      );

      this.gridPos = {
        x: gridX,
        y: gridY,
      };

      this.$nextTick(() => {
        this.drawIndicators();
      });
    },
    handleTouchStart(e) {
      e.preventDefault();

      this.touchDown = true;

      const relativeX =
        e.touches[0].pageX - this.$refs.indicatorCanvas.offsetLeft - 9;
      const relativeY =
        e.touches[0].pageY - this.$refs.indicatorCanvas.offsetLeft - 9;

      const { x: gridX, y: gridY } = this.retrieveGridCoord(
        relativeX,
        relativeY
      );

      this.gridPos = {
        x: gridX,
        y: gridY,
      };

      this.gridFirstPos = {
        x: gridX,
        y: gridY,
      };

      this.$nextTick(() => {
        this.drawIndicators();
      });
    },
    handleTouchEnd(e) {
      e.preventDefault();

      this.touchDown = false;
      this.$nextTick(() => {
        this.drawIndicators();
      });
    },
    retrieveGridCoord(mouseX, mouseY) {
      const gridX = Math.floor(mouseX / this.gridSize);
      const gridY = Math.floor(mouseY / this.gridSize);

      return { x: gridX, y: gridY };
    },
    retrieveMouseCoord(gridX, gridY) {
      const mouseX = gridX * this.gridSize;
      const mouseY = gridY * this.gridSize;

      return { x: mouseX, y: mouseY };
    },
    drawSquare(gridX, gridY, width, height, color) {
      this.indicatorCtx.fillStyle = color;

      const { x, y } = this.retrieveMouseCoord(gridX, gridY);

      this.indicatorCtx.fillRect(
        x,
        y,
        this.gridSize * width,
        this.gridSize * height
      );

      for (let i = 0; i < width; i += 1) {
        for (let j = 0; j < height; j += 1) {
          const offsetX = x + this.gridSize * i;
          const offsetY = y + this.gridSize * j;

          this.indicatorCtx.strokeRect(
            offsetX,
            offsetY,
            this.gridSize,
            this.gridSize
          );
        }
      }
    },
    drawIndicators() {
      this.indicatorCtx.clearRect(0, 0, this.appWidth, this.appHeight);

      if (this.mouseDown || this.touchDown) {
        const drawStartX =
          this.gridPos.x >= this.gridFirstPos.x
            ? this.gridFirstPos.x
            : this.gridPos.x;
        const drawStartY =
          this.gridPos.y >= this.gridFirstPos.y
            ? this.gridFirstPos.y
            : this.gridPos.y;

        const diffX = Math.abs(this.gridPos.x - this.gridFirstPos.x) + 1;
        const diffY = Math.abs(this.gridPos.y - this.gridFirstPos.y) + 1;

        this.drawSquare(drawStartX, drawStartY, diffX, diffY, "yellow");
      } else {
        this.drawSquare(this.gridPos.x, this.gridPos.y, 1, 1, "blue");
      }
    },
  },
  mounted() {
    const indicatorCanvas = this.$refs.indicatorCanvas;
    this.indicatorCtx = indicatorCanvas.getContext("2d");
  },
};
</script>

<style>
#indicatorCanvas {
  position: absolute;
  z-index: 20;
  cursor: crosshair;
}
</style>