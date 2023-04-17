<script setup lang="ts">
import { defineProps, ref } from "vue";
import { IPlayer } from "./Game.vue";

interface IChildProps {
  objects: IPlayer[];
}

const props = defineProps<IChildProps>();
const players = ref<IPlayer[]>(props.objects);

// let board = ref(JSON.parse(localStorage.getItem("board") || "[]"))

const emit = defineEmits(["showScore", "startOver"]);

const currentPlayerIndex = ref(0);

const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const calculateWinner = (squares) => {
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
};

const winner = ref<string | null>(null);

const makeMove = (x: number, y: number) => {
  if (winner.value || board.value[x][y] !== "") return;

  const current = players.value[currentPlayerIndex.value];

  board.value[x][y] = current.character;
//   saveBoardToLS(board.value)

  const win = calculateWinner(board.value.flat());
  if (win) {
    current.score++;
    winner.value = current.name;
  } else if (isBoardFull()) {
    winner.value = "tie";
  } else {
    currentPlayerIndex.value =
      (currentPlayerIndex.value + 1) % players.value.length;
  }
};

const isBoardFull = () => {
  return board.value.every((row) => row.every((cell) => cell !== ""));
};

const resetGame = () => {
  winner.value = null;
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
//   saveBoardToLS(board.value);
  currentPlayerIndex.value = 0;
};

const startOver = () => {
   players.value = [];
   resetGame();
   emit("startOver");
}

const showHighScore = () => {
    emit("showScore", players.value);
}

// function saveBoardToLS(boardie: string[][]) {
//   localStorage.setItem("board", JSON.stringify(boardie));
// }
// function savePlayersToLS(players: IPlayer[]) {
//   localStorage.setItem("players", JSON.stringify(players));
// }

</script>

<template>
    <div v-if="players.length > 1">
      <h2>{{ players[currentPlayerIndex].name }}'s turn</h2>
      <div class="board">
        <div v-for="(row, x) in board" :key="x" class="cell">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="makeMove(x, y)"
            class="square"
          >
          {{ cell && cell !== "" ? cell : "" }}
          </div>
        </div>
      </div>
      <div>
        <h2 v-show="winner">{{ winner }} wins!</h2>
        <h2 v-if="!winner && isBoardFull()">It's a tie!</h2>
        <button @click="resetGame">Reset Game</button>
        <button @click="startOver">Start Over</button>
        <button @click="showHighScore">Show High Score</button>
      </div>
    </div>
  </template>

 
<style scoped>
.board{
    display: flex;
    justify-content: center;
    align-items: center;
    
}

button{
    margin: .5rem;
}

.square{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 100px;
    border: solid 1px black;
    font-size: 30px;
}

</style>