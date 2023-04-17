<script setup lang="ts">
import Players from "./Players.vue";
import Board from "./Board.vue";
import Highscore from "./Highscore.vue"
import { ref } from 'vue'

export interface IPlayer {
    name: string
    score: number
    character: string
}

interface IChildProps {
  objects: IPlayer[];
}

// const emit = defineEmits(["startGame"]);

// let playerArr = ref<IPlayer[]>(JSON.parse(localStorage.getItem("players") || "[]"))

const playerArr =  ref<IPlayer[]>([]);
const scoreArr = ref<IPlayer[]>([]);

const savePlayers = (newPlayers: IPlayer[]) => {
    for(let i = 0; i < newPlayers.length; i++) {
        // playerArr.value = [];
        playerArr.value.push(newPlayers[i]);
        // savePlayersToLS(playerArr.value);
    }
    playerArr.value = [];
}

const showLeaderBoard = (fromGame: IPlayer[]) => {
    for(let i = 0; i < fromGame.length; i++) {
        scoreArr.value.push(fromGame[i]);
    }
    scoreArr.value = [];
}

// function savePlayersToLS(players: IPlayer[]) {
//   localStorage.setItem("players", JSON.stringify(players));
// }

</script>

<template>
    <Board :objects="playerArr" @show-score="showLeaderBoard"></Board>
    <Players @store-players=savePlayers></Players>
    <Highscore :objects="scoreArr"></Highscore>
</template>

<style scoped>

</style>