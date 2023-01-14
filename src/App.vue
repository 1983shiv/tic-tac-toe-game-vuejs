<script>
import Square from './components/square.vue'



export default {
  name: "app",
  components: { Square },
  computed: {},
  data() {
    return {
      xIsNext: true,
      restartGame: false,
      squareValue: Array(9).fill(null),
      status: "",
      playCount: localStorage.getItem('playCount') ? parseInt(localStorage.getItem('playCount')) : 1,
      lines: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ]
    }
  },
  computed: {

  },
  methods: {
    computeStatus() {
      if (this.calculateWinner()) {
        this.status = "Winner: " + this.calculateWinner();
      } else {
        this.status = "Next player: " + (this.xIsNext ? "X" : "O");
      }
      return this.status;
    },
    calculateWinner() {
      for (let i = 0; i < this.lines.length; i++) {
        const [a, b, c] = this.lines[i];
        // console.log("a, b, c", a, b, c, this.squareValue)
        if (
          this.squareValue[a] &&
          this.squareValue[a] === this.squareValue[b] &&
          this.squareValue[a] === this.squareValue[c]
        ) {
          return this.squareValue[a];
        }
      }
      return null;
    },
    onActionFill(i) {
      if (this.squareValue[i]) {
        return
      }
      if (this.calculateWinner()) {
        localStorage.setItem("playCount", this.playCount);
        this.restartGame = true
        return
      }

      if (this.xIsNext) {
        this.squareValue[i] = "X"
      } else {
        this.squareValue[i] = "O"
      }
      if (this.calculateWinner()) {
        localStorage.setItem("playCount", this.playCount);
        this.restartGame = true
        return
      }
      this.setXIsNext()
      return this.squareValue
    },
    setXIsNext() {
      this.xIsNext = !(this.xIsNext);
    },
    newGame() {
      this.playCount = this.playCount + 1;
      localStorage.setItem("playCount", this.playCount);
      this.squareValue = Array(9).fill(null);
      this.restartGame = false;
      this.xIsNext = true;
    }
  },

}
</script>

<template>
  <div class="centerpage">
    <h1>Tic Tac Toe Game</h1>
    <p class="showStatus">{{ computeStatus() }}</p>
    <button class="restartButton" v-if="restartGame" @click="newGame">Restart Game</button>
    <div class="row">
      <Square :squareValue="squareValue[0]" @actionFill="onActionFill(0)" />
      <Square :squareValue="squareValue[1]" @actionFill="onActionFill(1)" />
      <Square :squareValue="squareValue[2]" @actionFill="onActionFill(2)" />
    </div>
    <div class="row">
      <Square :squareValue="squareValue[3]" @actionFill="onActionFill(3)" />
      <Square :squareValue="squareValue[4]" @actionFill="onActionFill(4)" />
      <Square :squareValue="squareValue[5]" @actionFill="onActionFill(5)" />
    </div>
    <div class="row">
      <Square :squareValue="squareValue[6]" @actionFill="onActionFill(6)" />
      <Square :squareValue="squareValue[7]" @actionFill="onActionFill(7)" />
      <Square :squareValue="squareValue[8]" @actionFill="onActionFill(8)" />
    </div>
  </div>
  <div class="bottompage">Play Count : {{ playCount }} | Source Code : <a
      href="https://github.com/1983shiv/tic-tac-toe-game-vuejs">Github</a></div>
</template>


<style scoped>
.centerpage {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.bottompage {
  position: absolute;
  bottom: 0;
  /* margin-bottom: 10px; */
  padding: 20px 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  text-align: center;
  /* padding: 4px 4px; */
  background-color: #f4f4f4;
}

.square {
  background-color: #fff;
  width: 80px;
  height: 80px;
  border: 1px solid #61dafb;
  text-align: center;
  font-weight: bold;
  font-size: 32px;
  line-height: 80px;
}

.showStatus {
  background-color: #f6f6f6;
  padding: 6px 10px;
  text-align: center;
}

.restartButton {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-bottom: 10px;
  padding: 8px 8px;
  background-color: #f6f6f6;
  border: none;
  font-size: 20px;
  font-weight: bold;
  color: green;
}

.row {
  display: flex;
  justify-content: center;
}
</style>