<script setup lang="ts">

const checkBoard = () =>{

}

</script>

<template>

<div class="board-container">

    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
</div>
    
</template>

<style scoped>

.square {
    border: 1px solid black;
    width: 100px;
    height: 100px;
    flex-basis: 33%;
}
.board-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
}

</style>