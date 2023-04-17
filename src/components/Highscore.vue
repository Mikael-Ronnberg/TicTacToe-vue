<script setup lang="ts">
import { ref, computed } from 'vue';
import { IPlayer } from "./Game.vue";

interface IChildProps {
  objects: IPlayer[];
}

const props = defineProps<IChildProps>();

const players = ref<IPlayer[]>(props.objects);

  const leader = computed(() => {
  let highestScore = 0;
  let winner = '';
  for (let i = 0; i < players.value.length; i++) {
    if (players.value[i].score > highestScore) {
      highestScore = players.value[i].score;
      winner = players.value[i].name;
    }
  }
  return winner;
});

</script>

<template>
  <div>
    <p v-if="leader">Current Leader: {{ leader }}</p>
    <ul>
      <li v-for="player in players">
        <p>{{ player.name }} : {{ player.score }}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>

li{
  list-style: none;
}

</style>