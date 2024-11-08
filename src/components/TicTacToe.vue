<script setup lang="ts">
import { ref } from 'vue';

const grid = ref(Array(9).fill(""));
const currentPlayer = ref("X");
const playerX = ref("");
const playerO = ref("");
const namesConfirmed = ref(false);
const winner = ref<string | null>(null); // <string | null 
const draw = ref(false);

const confirmNames = () => {
  if (playerX.value && playerO.value) {
    namesConfirmed.value = true;
  } else {
    alert('Please enter names for PlayerX and PlayerO.')
  }
};

const handleClick = (i: number) => {
  if (!winner.value && !draw.value && grid.value[i] === "") {
    grid.value[i] = currentPlayer.value;
    checkWinner();
    currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
  }
};

const winningCombos = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8],
  [0, 3, 6], [1, 4, 7], [2, 5, 8],
  [0, 4, 8], [2, 4, 6]
];

const checkWinner = () => {
  for (const [a, b, c] of winningCombos) {
    if (
      grid.value[a] &&
      grid.value[a] === grid.value[b] &&
      grid.value[a] === grid.value[c]
    ) {
      winner.value =  grid.value[a] === "X" ? playerX.value : playerO.value;
      return;
    }
  };
  if (grid.value.every(cell => cell !== "")) {
  draw.value = true;
  };
};

const resetGame = () => {
  grid.value = Array(9).fill("");
  currentPlayer.value = "X";
  playerX.value = "";
  playerO.value = "";
  namesConfirmed.value = false;
  winner.value = null;
  draw.value = false;
}

</script>


<template>
  <div v-if="!namesConfirmed" class="add-players">
    <label class="player-names">
      Player X:
      <input v-model="playerX" />
    </label>
    <label class="player-names">
      Player O:
      <input v-model="playerO" />
    </label>
    <button @click="confirmNames">Let's Play</button>
  </div>

  <div v-if="namesConfirmed" class="game-container">
    <p v-if="!winner && !draw" class="player-turn">{{ currentPlayer === "X" ? playerX : playerO }}'s turn </p>
    <p v-else-if="winner" class="winner-msg">{{ winner }} wins</p>
    <p v-else="draw && !winner" class="draw-msg">It's a draw</p>

    <div class="grid-container">
      <div class="grid">
        <div v-for="(cell, i) in grid" :key="i" class="cell" @click="handleClick(i)"
        :class="{'cell-disabled' : winner}" >
          {{ cell }}
        </div>
      </div>
    </div>

    <button @click="resetGame">Reset Game</button>
  </div>
</template>


<style scoped>
.player-names {
  display: block;
  font-size: 1.2rem;
  font-weight: 600;
}

input {
  font-size: 1.3rem;
  background-color: #ffefcd;
  border: 2px solid #342b25;
  border-radius: 6px;
  margin: 5px;
}

button {
  color: #342b25;
  background-color:#cdede8;
  margin: 20px;
  border: 2px solid #5a4b42;
}

.player-turn {
  font-size: 1.2rem;
  font-weight: 600;
}

.grid-container {
  display: flex;
  justify-content: center;
}

.grid {

  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  width: 230px;
  border: 10px solid #5a4b42;
  border-radius: 9px;
  margin: 10px;
  background-color: #5a4b42;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 70px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 2rem; 
  font-weight: bold;
  color: #184550;
  background-color: #ffefcd;
}

</style>