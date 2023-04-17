<script setup lang="ts">
import { ref } from "vue";
import { IPlayer } from "./Game.vue";

let playerX = ref("");
let playerO = ref("");
let showX = ref(true);
let showO = ref(false);

const playerArr =  ref<IPlayer[]>([]);

const startGame = () => {
    showX.value = true;
    playerArr.value = [];
}

const handleX = () => {
    let playerOne: IPlayer = {
        name: playerX.value, 
        score: 0,
        character: "X"
    }
    playerArr.value.push(playerOne);
    playerX.value = "";
    showX.value = false;
    showO.value = true;
    console.log(playerArr.value)
}
const handleO = () => {
    let playerTwo: IPlayer = {
        name: playerO.value,
        score: 0,
        character: "O"
    }
    playerArr.value.push(playerTwo);
    emit("storePlayers", playerArr.value);
    playerO.value = "";
    showO.value = false;
}

const emit = defineEmits(["storePlayers"]);

// startGame();

</script>

<template>
    <form @submit.prevent v-show="showX">
        <p>Player X</p>
        <input type="text" v-model="playerX">
        <button @click="handleX">Save</button>
    </form>

    <form @submit.prevent v-show="showO">
        <p>Player O</p>
        <input type="text" v-model="playerO">
        <button @click="handleO">Start The Game</button>
    </form>
</template>

<style scoped>


</style>