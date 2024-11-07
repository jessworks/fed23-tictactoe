<script setup lang="ts">
import { ref } from 'vue';

const grid = ref(Array(9).fill(""));
const currentPlayer = ref("X");
const playerX = ref("");
const playerO = ref("");
const namesConfirmed = ref(false);

const confirmNames = () => {
  if (playerX.value && playerO.value) {
    namesConfirmed.value = true;
  } else {
    alert('Please enter names for PlayerX and PlayerO.')
  }
};

const handleClick = (i: number) => {
  console.log(`cell ${i} clicked`);

  if (grid.value[i] === "") {
    grid.value[i] = currentPlayer.value;
    currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
  }
};

const resetGame = () => {
  grid.value = Array(9).fill("");
  currentPlayer.value = "X";
  playerX.value = "";
  playerO.value = "";
  namesConfirmed.value = false;
}

</script>


<template>
  <div v-if="!namesConfirmed" class="add-players">
    <label>
      Player X:
      <input v-model="playerX" />
    </label>
    <label>
      Player O:
      <input v-model="playerO" />
    </label>
    <button @click="confirmNames">Let's play</button>
  </div>

  <p v-if="namesConfirmed">{{ currentPlayer === "X" ? playerX : playerO }}'s turn </p>

  <div class="grid">
    <div v-for="(cell, i) in grid" :key="i" class="cell" @click="handleClick(i)">
      {{ cell }}
    </div>
  </div>

  <button @click="resetGame">reset game</button>

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