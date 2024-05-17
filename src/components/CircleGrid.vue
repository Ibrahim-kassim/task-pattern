<template>
  <div class="container">
    <div
    class="circle-grid"
    @mousedown="handleMouseDown"
    @mousemove="handleMouseMove"
    @mouseup="handleMouseUp"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd"
  >
    <div
      class="circle"
      v-for="(value, index) in circleValues"
      :key="index"
      :data-value="value"
      :class="{ active: isActiveCircle(value) }"
    >
      {{ value }}
    </div>
    <canvas ref="canvas" class="canvas"></canvas>

    <button @click="resetPattern">Reset</button>
    <div>{{ patternNumbers.join(' ') }}</div>
  </div>
  </div>
</template>

<script>
export default {
  name: 'CircleGrid',
  data() {
    return {
      circleValues: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      patternNumbers: [],
      touching: false,
      mouseDown: false,
    };
  },
  mounted() {
    this.initializeCanvas();
  },
  methods: {
    handleTouchStart(event) {
      this.touching = true;
      this.handleTouchMove(event); // Handle initial touch
    },
    handleTouchMove(event) {
      if (!this.touching) return;
      const touch = event.touches[0];
      const element = document.elementFromPoint(touch.clientX, touch.clientY);
      if (element && element.classList.contains('circle')) {
        const value = parseInt(element.dataset.value, 10);
        this.handleCircleClick(value);
      }
    },
    handleTouchEnd() {
      this.touching = false;
    },
    handleMouseDown(event) {
      this.mouseDown = true;
      this.handleMouseMove(event); // Handle initial click
    },
    handleMouseMove(event) {
      if (!this.mouseDown) return;
      const element = document.elementFromPoint(event.clientX, event.clientY);
      if (element && element.classList.contains('circle')) {
        const value = parseInt(element.dataset.value, 10);
        this.handleCircleClick(value);
      }
    },
    handleMouseUp() {
      this.mouseDown = false;
    },
    handleCircleClick(value) {
      if (this.patternNumbers.length > 0) {
        const lastValue = this.patternNumbers[this.patternNumbers.length - 1];
        const intermediateValues = this.getIntermediateValues(lastValue, value);
        intermediateValues.forEach((val) => {
          if (!this.patternNumbers.includes(val)) {
            this.patternNumbers.push(val);
          }
        });
      } else if (!this.patternNumbers.includes(value)) {
        this.patternNumbers.push(value);
      }
      if (!this.patternNumbers.includes(value)) {
        this.patternNumbers.push(value);
      }
      console.log(`Clicked circle with value: ${value}`);
      console.log('Current pattern numbers:', this.patternNumbers);
      this.drawLine();
    },
    resetPattern() {
      this.patternNumbers = [];
      this.clearCanvas();
      console.log('Pattern numbers reset to empty');
    },
    isActiveCircle(value) {
      return this.patternNumbers.includes(value);
    },
    initializeCanvas() {
      const canvas = this.$refs.canvas;
      canvas.width = 360; // Adjust canvas width
      canvas.height = 360; // Adjust canvas height
      this.clearCanvas();
    },
    clearCanvas() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);
    },
    drawLine() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      this.clearCanvas();

      if (this.patternNumbers.length < 2) return;

      ctx.beginPath();
      const getPosition = (value) => {
        const index = this.circleValues.indexOf(value);
        const row = Math.floor(index / 3);
        const col = index % 3;
        const x = col * 120 + 60; // Adjust for padding and circle size
        const y = row * 120 + 60; // Adjust for padding and circle size
        return { x, y };
      };

      const startPos = getPosition(this.patternNumbers[0]);
      ctx.moveTo(startPos.x, startPos.y);

      for (let i = 1; i < this.patternNumbers.length; i++) {
        const pos = getPosition(this.patternNumbers[i]);
        ctx.lineTo(pos.x, pos.y);
      }

      ctx.strokeStyle = '#2ecc71';
      ctx.lineWidth = 2;
      ctx.stroke();
    },
    getIntermediateValues(start, end) {
      const startIndex = this.circleValues.indexOf(start);
      const endIndex = this.circleValues.indexOf(end);
      const intermediateValues = [];

      // Calculate row and column of the start and end points
      const startRow = Math.floor(startIndex / 3);
      const startCol = startIndex % 3;
      const endRow = Math.floor(endIndex / 3);
      const endCol = endIndex % 3;

      // Check if the move is along a straight line (horizontal, vertical, diagonal)
      if (startRow === endRow) {
        // Horizontal line
        const minCol = Math.min(startCol, endCol);
        const maxCol = Math.max(startCol, endCol);
        for (let col = minCol + 1; col < maxCol; col++) {
          intermediateValues.push(this.circleValues[startRow * 3 + col]);
        }
      } else if (startCol === endCol) {
        // Vertical line
        const minRow = Math.min(startRow, endRow);
        const maxRow = Math.max(startRow, endRow);
        for (let row = minRow + 1; row < maxRow; row++) {
          intermediateValues.push(this.circleValues[row * 3 + startCol]);
        }
      } else if (Math.abs(endRow - startRow) === Math.abs(endCol - startCol)) {
        // Diagonal line
        const rowStep = endRow > startRow ? 1 : -1;
        const colStep = endCol > startCol ? 1 : -1;
        let row = startRow + rowStep;
        let col = startCol + colStep;
        while (row !== endRow && col !== endCol) {
          intermediateValues.push(this.circleValues[row * 3 + col]);
          row += rowStep;
          col += colStep;
        }
      }

      // Always include the end value as well
      intermediateValues.push(end);

      return intermediateValues;
    },
  },
};
</script>

<style scoped>
.circle-grid {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-gap: 20px;
  position: relative; /* Position relative for absolute positioning of lines */
  width: 360px;
}

.circle {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background-color: #3498db;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Smooth transition for background color */
}

.circle.active {
  background-color: #2ecc71; 
}

.canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none; 
/*  */
}

</style>
