<script setup lang="ts">
import { ref } from 'vue';

const grid = ref(Array(9).fill(""));
const currentPlayer = ref("X");

function handleClick(i: number) {
  console.log(`cell ${i} clicked`);

  if (grid.value[i] === "") {
    grid.value[i] = currentPlayer.value;
    currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
  }
};

</script>


<template>
  <div class="grid">
    <div v-for="(cell, i) in grid" :key="i" class="cell" @click="handleClick(i)">
      {{ cell }}
    </div>
  </div>

</template>


<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  width: 320px;
  border: 10px solid blueviolet;
  background-color: blueviolet;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100px;
  border: 1px solid #ccc;
  cursor: pointer;
  font-size: 18px;
  background-color: aqua;
}

</style>