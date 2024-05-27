<template>
  <div id="app">
    <div class="road-crossing">
      <div class="vertical-line left"></div>
      <div class="vertical-line center"></div>
      <div class="vertical-line right"></div>
      <div class="horizontal-line top"></div>
      <div class="horizontal-line center"></div>
      <div class="horizontal-line bottom"></div>

      <!-- Circles at the corners -->
      <div :class="['circle', 'top-left', topLeftBottomRightColor]"></div>
      <div :class="['circle', 'top-right', topRightBottomLeftColor]"></div>
      <div :class="['circle', 'bottom-left', topRightBottomLeftColor]"></div>
      <div :class="['circle', 'bottom-right', topLeftBottomRightColor]"></div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      topLeftBottomRightColor: 'red',
      topRightBottomLeftColor: 'red',
      updateInterval: null,
    };
  },
  created() {
    this.updateColors();
  },
  methods: {
    async updateColors() {
      try {
        const response = await axios.get('http://localhost:5001/color');
        this.topLeftBottomRightColor = response.data.topLeftBottomRight;
        this.topRightBottomLeftColor = response.data.topRightBottomLeft;
        
        // Clear any existing interval
        if (this.updateInterval) {
          clearInterval(this.updateInterval);
        }
        
        // Set the next update based on the remaining time
        this.updateInterval = setTimeout(this.updateColors, response.data.remainingTime * 1000);
      } catch (error) {
        console.error('Error fetching color:', error);
      }
    }
  }
};
</script>

<style>
#app {
  text-align: center;
  position: relative;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}

.road-crossing {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 100%;
}

.vertical-line, .horizontal-line {
  position: absolute;
  background-color: black;
}

.vertical-line {
  width: 3px;
  height: 100%;
}

.vertical-line.left {
  left: calc(50% - 60px);
}

.vertical-line.center {
  left: 50%;
  transform: translateX(-50%);
}

.vertical-line.right {
  left: calc(50% + 60px);
}

.horizontal-line {
  height: 3px;
  width: 100%;
}

.horizontal-line.top {
  top: calc(50% - 60px);
}

.horizontal-line.center {
  top: 50%;
  transform: translateY(-50%);
}

.horizontal-line.bottom {
  top: calc(50% + 60px);
}

.circle {
  position: absolute;
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.circle.red {
  background-color: red;
}

.circle.green {
  background-color: green;
}

.circle.yellow {
  background-color: yellow;
}

.circle.top-left {
  top: calc(50% - 60px - 25px); /* Adjusted for circle size */
  left: calc(50% - 60px - 25px); /* Adjusted for circle size */
}

.circle.top-right {
  top: calc(50% - 60px - 25px); /* Adjusted for circle size */
  left: calc(50% + 60px - 25px); /* Adjusted for circle size */
}

.circle.bottom-left {
  top: calc(50% + 60px - 25px); /* Adjusted for circle size */
  left: calc(50% - 60px - 25px); /* Adjusted for circle size */
}

.circle.bottom-right {
  top: calc(50% + 60px - 25px); /* Adjusted for circle size */
  left: calc(50% + 60px - 25px); /* Adjusted for circle size */
}
</style>

