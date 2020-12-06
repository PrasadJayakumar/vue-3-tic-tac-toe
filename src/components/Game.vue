<template>
  <div className="game">
    <div className="game-board">
      <board :squares="current.squares" @click="handleClick" />
    </div>

    <div className="game-info">
      <div>{{ status }}</div>
      <ol id="time-travel">
        <li v-for="(ignore, move) in current.historySize" :key="move">
          <button @click="jumpTo(move)">
            {{ move ? "Go to move #" + move : "Go to game start" }}
          </button>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
import { reactive, ref, watch } from "vue";
import Board from "./Board.vue";

export default {
  components: {
    Board,
  },
  setup() {
    let history = [{ squares: Array(9).fill(null) }];

    // Reactive variables
    let current = reactive({
      squares: [...history[0].squares],
      xIsNext: true,
      stepNumber: 0,
      historySize: 1,
    });
    let status = ref("Next player: " + (current.xIsNext ? "X" : "O"));

    // Reactivity
    watch(
      current,
      () => {
        let winner = calculateWinner(current.squares);
        if (winner) {
          status.value = "Winner: " + winner;
        } else {
          status.value = "Next player: " + (current.xIsNext ? "X" : "O");
        }
      },
      { immediate: true }
    );

    // Methods
    function handleClick(i) {
      history = history.slice(0, current.stepNumber + 1);

      let last = history[history.length - 1];
      if (calculateWinner(last.squares) || last.squares[i]) return;

      let squares = [...last.squares];
      squares[i] = current.xIsNext ? "X" : "O";
      history.push({ squares });

      current.stepNumber = history.length;
      current.xIsNext = !current.xIsNext;
      current.squares = squares;
      current.historySize = history.length;
    }

    function jumpTo(step) {
      current.stepNumber = step;
      current.xIsNext = step % 2 === 0;
      current.squares = history[step].squares;
    }

    function calculateWinner(squares) {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          squares[a] &&
          squares[a] === squares[b] &&
          squares[a] === squares[c]
        ) {
          return squares[a];
        }
      }
      return null;
    }

    return {
      current,
      status,
      handleClick,
      jumpTo,
    };
  },
};
</script>
