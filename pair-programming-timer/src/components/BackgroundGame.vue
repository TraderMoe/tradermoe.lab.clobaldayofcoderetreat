<template>
  <div class="game-of-life">
    <h2>Iterations: {{ iterations }}</h2>
    <canvas id="canvas"></canvas>
    <div class="controls">
      <button @click="runAutoplay">{{ autoPlayEnabled ? "⏸️" : "▶️" }}</button>
      <button @click="evaluateNextGeneration">⏭️</button>
      <button @click="randomizeBoard">🔄️</button>
      <button @click="initializeGliders">✈️</button>
      <button @click="initializeGliderGun">✈️🔫</button>
      <button @click="initializeClearBoardSequence">🧼</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    boardSize: {
      type: Number,
      default: 50,
      required: false,
    },
  },
  methods: {
    countNeighbors(grid, x, y) {
      let sum = 0;
      for (let i = -1; i <= 1; i++) {
        for (let j = -1; j <= 1; j++) {
          let row = x + i;
          let col = y + j;
          if (
            row >= 0 &&
            row < this.boardSize &&
            col >= 0 &&
            col < this.boardSize
          ) {
            sum += grid[row][col];
          }
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
      this.iterations++;
      return newGrid;
    },
    drawGrid() {
      let canvas = this.canvas;
      let ctx = this.canvasContext;
      let grid = this.grid;

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
            ctx.fillStyle = "#202020";
          }

          // Draw the cell
          ctx.fillRect(
            j * cellWidth,
            i * cellHeight,
            cellWidth * 0.95,
            cellHeight * 0.95
          );
        }
      }
    },
    evaluateNextGeneration() {
      this.grid = this.nextGeneration(this.grid);
      this.drawGrid();
    },
    runAutoplay() {
      this.autoPlayEnabled = !this.autoPlayEnabled;
      if (this.autoPlayEnabled) {
        this.autoPlayInterval = setInterval(() => {
          this.evaluateNextGeneration();
        }, 100);
      } else {
        clearInterval(this.autoPlayInterval);
      }
    },
    randomizeBoard() {
      this.setAndDrawGrid(() => {
        for (let i = 0; i < this.boardSize; i++) {
          for (let j = 0; j < this.boardSize; j++) {
            this.grid[i][j] = Math.floor(Math.random() * 2);
          }
        }
      });
    },
    initializeGliders() {
      this.setAndDrawGrid(() => {
        this.grid[10][11] = 1;
        this.grid[11][12] = 1;
        this.grid[12][10] = 1;
        this.grid[12][11] = 1;
        this.grid[12][12] = 1;

        this.grid[10][1] = 1;
        this.grid[11][2] = 1;
        this.grid[12][0] = 1;
        this.grid[12][1] = 1;
        this.grid[12][2] = 1;

        this.grid[0][10] = 1;
        this.grid[1][12] = 1;
        this.grid[2][10] = 1;
        this.grid[2][11] = 1;
        this.grid[2][12] = 1;
      });
    },
    initializeClearBoardSequence() {
      this.setAndDrawGrid(() => {
        this.grid[21][23] = 1;
        this.grid[21][24] = 1;
        this.grid[21][25] = 1;
        this.grid[22][23] = 1;
        this.grid[23][23] = 1;
        this.grid[22][25] = 1;
        this.grid[23][25] = 1;

        this.grid[25][23] = 1;
        this.grid[26][23] = 1;
        this.grid[27][23] = 1;
        this.grid[27][24] = 1;
        this.grid[27][25] = 1;
        this.grid[26][25] = 1;
        this.grid[25][25] = 1;
      });
    },
    initializeGliderGun() {
      this.setAndDrawGrid(() => {
        //left square
        this.grid[5][1] = 1;
        this.grid[5][2] = 1;
        this.grid[6][1] = 1;
        this.grid[6][2] = 1;

        //left side
        this.grid[3][13] = 1;
        this.grid[3][14] = 1;
        this.grid[4][12] = 1;
        this.grid[4][16] = 1;
        this.grid[5][11] = 1;
        this.grid[5][17] = 1;
        this.grid[6][11] = 1;
        this.grid[6][15] = 1;
        this.grid[6][17] = 1;
        this.grid[6][18] = 1;
        this.grid[7][11] = 1;
        this.grid[7][17] = 1;
        this.grid[8][12] = 1;
        this.grid[8][16] = 1;
        this.grid[9][13] = 1;
        this.grid[9][14] = 1;

        //right side
        this.grid[1][25] = 1;
        this.grid[2][23] = 1;
        this.grid[2][25] = 1;
        this.grid[3][21] = 1;
        this.grid[3][22] = 1;
        this.grid[4][21] = 1;
        this.grid[4][22] = 1;
        this.grid[5][21] = 1;
        this.grid[5][22] = 1;
        this.grid[6][23] = 1;
        this.grid[6][25] = 1;
        this.grid[7][25] = 1;

        //right square
        this.grid[3][35] = 1;
        this.grid[3][36] = 1;
        this.grid[4][35] = 1;
        this.grid[4][36] = 1;
      });
    },
    setAndDrawGrid(setData) {
      this.setEmptyGrid();
      setData();
      this.drawGrid();
    },
    setEmptyGrid() {
      this.iterations = 0;
      this.grid = new Array(this.boardSize);
      for (let i = 0; i < this.boardSize; i++) {
        this.grid[i] = new Array(this.boardSize);
      }

      for (let i = 0; i < this.boardSize; i++) {
        for (let j = 0; j < this.boardSize; j++) {
          this.grid[i][j] = 0;
        }
      }
    },
  },
  data() {
    return {
      canvas: null,
      canvasContext: null,
      grid: null,
      autoPlayEnabled: false,
      autoPlayInterval: null,
      iterations: 0,
    };
  },
  mounted() {
    // Create the initial grid
    this.setEmptyGrid();

    this.canvas = document.getElementById("canvas");
    canvas.width = 450;
    canvas.height = 450;
    this.canvasContext = this.canvas.getContext("2d");

    this.randomizeBoard();

    // Draw the initial grid
    this.drawGrid();
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

.controls {
  max-width: 500px;
}
</style>
