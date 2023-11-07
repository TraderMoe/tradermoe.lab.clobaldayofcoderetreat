<template>
  <div class="game-of-life">
    <canvas id="canvas"></canvas>
    <div class="controls">
        <button @click="runAutoplay">⏯️</button>
        <button @click="evaluateNextGeneration">⏭️</button>
      <button @click="randomizeBoard">🔄️</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    boardSize: {
      type: Number,
      required: true,
    },
  },
  methods: {
    countNeighbors(grid, x, y) {
      let sum = 0;
      for (let i = -1; i <= 1; i++) {
        for (let j = -1; j <= 1; j++) {
          let row = (x + i + this.boardSize) % this.boardSize;
          let col = (y + j + this.boardSize) % this.boardSize;
          sum += grid[row][col];
        }
      }
      sum -= grid[x][y];
      return sum;
    },
    nextGeneration(grid) {
      let newGrid = new Array(this.boardSize);
      for (let i = 0; i < this.boardSize; i++) {
        newGrid[i] = new Array(this.boardSize);
      }

      // Loop through every cell in the grid
      for (let i = 0; i < this.boardSize; i++) {
        for (let j = 0; j < this.boardSize; j++) {
          // Count the number of live neighbors for this cell
          let neighbors = this.countNeighbors(grid, i, j);

          // Apply the rules of the game
          if (grid[i][j] == 0 && neighbors == 3) {
            newGrid[i][j] = 1;
          } else if (grid[i][j] == 1 && (neighbors < 2 || neighbors > 3)) {
            newGrid[i][j] = 0;
          } else {
            newGrid[i][j] = grid[i][j];
          }
        }
      }

      // Return the new grid
      return newGrid;
    },
    drawGrid(grid, canvas, ctx) {
      // Clear the canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Set the size of each cell
      let cellWidth = canvas.width / this.boardSize;
      let cellHeight = canvas.height / this.boardSize;

      // Loop through every cell in the grid
      for (let i = 0; i < this.boardSize; i++) {
        for (let j = 0; j < this.boardSize; j++) {
          // Set the color of the cell based on its value
          if (grid[i][j] == 1) {
            ctx.fillStyle = "#1E5031";
          } else {
            ctx.fillStyle = "transparent";
          }

          // Draw the cell
          ctx.fillRect(
            j * cellWidth,
            i * cellHeight,
            cellWidth * 0.99,
            cellHeight * 0.99
          );
        }
      }
    },
    evaluateNextGeneration() {
      this.grid = this.nextGeneration(this.grid);
      this.drawGrid(this.grid, this.canvas, this.canvasContext);
    },
    runAutoplay() {
      this.autoPlayEnabled = !this.autoPlayEnabled;
      if (this.autoPlayEnabled) {
        this.autoPlayInterval = setInterval(() => {
          this.grid = this.nextGeneration(this.grid);
          this.drawGrid(this.grid, this.canvas, this.canvasContext);
        }, 100);
      } else {
        clearInterval(this.autoPlayInterval);
      }
    },
    randomizeBoard() {
      for (let i = 0; i < this.boardSize; i++) {
        for (let j = 0; j < this.boardSize; j++) {
          this.grid[i][j] = Math.floor(Math.random() * 2);
        }
      }

      this.drawGrid(this.grid, this.canvas, this.canvasContext);
    },
  },
  data() {
    return {
      canvas: null,
      canvasContext: null,
      grid: null,
      autoPlayEnabled: false,
      autoPlayInterval: null,
    };
  },
  mounted() {
    // Create the initial grid
    this.grid = new Array(this.boardSize);
    for (let i = 0; i < this.boardSize; i++) {
      this.grid[i] = new Array(this.boardSize);
    }

    this.canvas = document.getElementById("canvas");
    canvas.width = 450;
    canvas.height = 450;
    this.canvasContext = this.canvas.getContext("2d");

    this.randomizeBoard();

    // Draw the initial grid
    this.drawGrid(this.grid, this.canvas, this.canvasContext);
  },
};
</script>

<style scoped>
.game-of-life {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-bottom: 5rem;
}
</style>
