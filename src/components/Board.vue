import VueSlider from "vue-slider-component"

<template>
  Conway's Game of Life
  <button @click="start">{{ started ? "Stop" : "Start" }}</button>
  <button @click="reset">Reset</button>
  <input type="range" min="50" max="2000" value="{{this.simulationSpeed}}" />
  <div v-for="(row, idxRow) in grid" :key="idxRow">
    <div
      id="cell"
      class="black"
      v-for="(col, idxCol) in row"
      :key="idxCol"
      :class="{ activecell: grid[idxRow][idxCol] }"
      @click="grid[idxRow][idxCol] = 1 - grid[idxRow][idxCol]"
    ></div>
  </div>
</template>

<script>
const numRows = 50;
const numCols = 100;
let rows = [];
for (let i = 0; i < numRows; i++) {
  rows.push(Array.from(Array(numCols), () => 0));
}

const neighborOperations = [
  [-1, -1],
  [-1, 0],
  [-1, 1],
  [0, -1],
  [0, 1],
  [1, -1],
  [1, 0],
  [1, 1],
];

export default {
  name: "Board",
  data() {
    return {
      grid: rows,
      started: false,
      simulationSpeed: 250,
    };
  },
  methods: {
    start() {
      this.started = !this.started;
      this.runSimulation();
    },
    runSimulation() {
      let total;
      let gridCopy = JSON.parse(JSON.stringify(this.grid));
      if (!this.started) return;
      else {
        for (let i = 0; i < numRows; i++) {
          for (let k = 0; k < numCols; k++) {
            total = 0;
            neighborOperations.forEach(([x, y]) => {
              total +=
                gridCopy[(i + x + numRows) % numRows][
                  (k + y + numCols) % numCols
                ];
            });
            if (total < 2 || total > 3) this.grid[i][k] = 0;
            if (total === 3) this.grid[i][k] = 1;
          }
        }
        setTimeout(() => {
          this.runSimulation();
        }, this.simulationSpeed);
      }
    },
    reset() {
      let rows = [];
      for (let i = 0; i < numRows; i++) {
        rows.push(Array.from(Array(numCols), () => 0));
      }
      this.grid = rows;
    },
    alternate() {
      for (let i = 0; i < numRows; i++) {
        for (let k = 0; k < numCols; k++) {
          //switching the values between 1 and 0
          if (this.grid[i][k]) this.grid[i][k] = 0;
          else this.grid[i][k] = 1;
        }
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
  height: 17px;
}
#cell {
  display: inline-block;
  height: 15px;
  width: 15px;
  margin: 1px;
  cursor: pointer;
}
.black {
  background-color: black;
  color: yellow;
}
.activecell {
  background-color: yellow;
  color: black;
}
</style>