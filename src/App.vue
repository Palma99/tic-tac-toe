<template>
  <div>
    <h1 class="title">Tic Tac Toe</h1>
    <div class="app">
      <div v-if="!winner.win" class="turn-container">
        <h1 class="turn">Turn:</h1>
        <h1 class="symbol-view">{{ turn ? "X" : "O" }}</h1>
      </div>
      <div v-if="winner.win" class="turn-container win-container">
        <h1 class="turn">{{ winner.player }} won!</h1>
        <button @click="restartGame" class="restart-button">Restart</button>
      </div>
      <div class="game-container">
        <div v-for="(cell, i) in cells" :key="i">
          <Cell
            :class="{ 'winning-cell': winningCells.includes(i) }"
            @nextTurn="changeTurn($event)"
            :turn="turn"
            :index="i"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Cell from "./components/Cell.vue";
function init() {
  return {
    cells: Array.from({ length: 9 }),
    winningCells: [],
    turn: true,
    winner: {
      win: false,
      player: ""
    },
    moves: 0
  };
}
export default {
  name: "App",
  components: {
    Cell
  },
  data: () => init(),
  methods: {
    /**
     * Restart the game. Reload page
     */
    restartGame() {
      location.reload();
    },

    /**
     * When click on a cell, change turn. Also check if game is finished
     * @param {number} index index of clicked cell
     */
    changeTurn(index) {
      /*
        turn:
          true  -> X
          false -> O
      */
      this.moves++;
      //console.log(this.moves);
      this.cells[index] = this.turn;
      const winner = this.checkForWinner();
      // If there is a winner
      if (winner !== -1) {
        this.winner.win = true;
        this.winner.player = winner ? "X" : "O";
      }

      // If board is full and no winner
      if (this.moves === 9 && winner === -1) {
        this.winner.player = "None";
        this.winner.win = true;
      }
      this.turn = !this.turn;
    },

    /**
     * Check if al element of a given set are equal.
     * Just checking if size is 1. Also check if the set contains
     * an undefined element.
     * @param {Set} elm
     * @returns {(number|boolean)} returns -1 if none won; in case of win
     *                            returns a true if x won, and false if o won
     */
    allEqual(elm) {
      if (elm.size === 1 && !elm.has(undefined)) {
        return elm.values().next().value;
      }
      return -1;
    },

    /**
     * Check every row, column and diagonal.
     * @returns {(number|boolean)} returns -1 if there is no winner, otherwise return a boolean:
     *                             true if X won, false if O won.
     */
    checkForWinner() {
      // Rows
      for (let i = 0; i < 9; i += 3) {
        // row contains a single row of the matrix
        let indexes = [i, i + 1, i + 2];
        const row = new Set(this.cells.slice(i, i + 3));
        const equal = this.allEqual(row);
        if (equal !== -1) {
          this.winningCells = [...indexes];
          return equal;
        }
      }

      // Cols
      for (let i = 0; i < 3; i++) {
        // col is a set which contains a column
        // i -> 0, 1, 2 is the number of column
        // j -> is the index of a cells; the filter returns:
        // return i + 3 === j || i + 6 === j || j === i

        let indexes = [i, i + 3, i + 6];
        const col = new Set(
          this.cells.filter((_, j) => {
            let elm = false;
            for (let k = 0; k < 3; k++) {
              elm = elm || i + 3 * k === j;
            }
            return elm;
          })
        );

        const equal = this.allEqual(col);
        if (equal !== -1) {
          this.winningCells = [...indexes];
          return equal;
        }
      }

      // Main diagonal
      let indexes = [0, 4, 8];
      const diag = new Set(
        indexes.map(i => {
          return this.cells[i];
        })
      );

      let equal = this.allEqual(diag);
      if (equal !== -1) {
        this.winningCells = [...indexes];
        return equal;
      }

      // Counterdiagonal
      indexes = [2, 4, 6];
      const cdiag = new Set(
        indexes.map(i => {
          return this.cells[i];
        })
      );

      equal = this.allEqual(cdiag);
      if (equal !== -1) {
        this.winningCells = [...indexes];
        return equal;
      }

      return -1;
    }
  }
};
</script>

<style lang="scss">
body {
  background: whitesmoke;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.title {
  font-size: 5em;
  text-align: center;
  margin-top: 0em;
  color: #ff595e;
}

.app {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}

.game-container {
  display: grid;
  grid-template-columns: repeat(3, 0.1fr);
  gap: 0;
  width: min-content;
  border-radius: 10px;
  padding: 1rem;
  margin: 0;
  box-shadow: 0px 13px 28px -10px rgba(0, 0, 0, 0.416);
}

.win-container {
  display: grid !important;
}

.restart-button {
  border-radius: 10px;
  font-size: 2em;
  width: auto;
  border: 1px solid #ffca3a;
  background: #ffca3ad6;
}

.restart-button:hover {
  cursor: pointer;
  background: #ffca3a;
}

.turn-container {
  display: inline-flex;
  align-items: center;
}

.turn {
  font-size: 5em;
}

.cross {
  color: #1982c4;
}

.circle {
  color: #8ac926;
}

.symbol-view {
  font-size: 5em;
  width: 1em;
  margin-left: 0.8rem;
}

.winning-cell {
  background: #ffca3a;
}
</style>
