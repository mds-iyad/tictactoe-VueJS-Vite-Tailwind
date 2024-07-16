<script setup>
import { ref, computed } from 'vue'

const currentPlayer = ref('X');
const gameBoard = ref([
  ['','',''],
  ['','',''],
  ['','','']
]);

const determineWinner = (cells) => {
  const winningCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let combo of winningCombinations) {
    const [a, b, c] = combo;
    if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
      return cells[a];
    }
  }
  return null;
}

const winner = computed(() => determineWinner(gameBoard.value.flat()));

const makeMove = (row, col) => {
  // Si quelqu'un a déjà gagné
  if (winner.value) return;

  // Si la case est déjà utilisée
  if (gameBoard.value[row][col] !== '') return;

  // Mettre à jour la case avec le joueur actuel
  gameBoard.value[row][col] = currentPlayer.value;

  // Changer de joueur
  currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
}

const resetGame = () => {
  gameBoard.value = [
    ['','',''],
    ['','',''],
    ['','','']
  ];
  currentPlayer.value = 'X';
}
</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-600 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

    <h3 class="text-xl mb-4"> Player {{ currentPlayer }}</h3>

    <div class="flex flex-col items-center mb-8">
      <div
        v-for="(row, rowIndex) in gameBoard"
        :key="rowIndex"
        class="flex">

        <div 
          v-for="(cell, colIndex) in row"
          :key="colIndex"
          @click="makeMove(rowIndex, colIndex)"
          :class="`border border-white w-20 h-20 hover:bg-gray-400 flex items-center 
          justify-center material-icons-outlined cursor-pointer ${ cell === 'X' ? 'text-pink-500' : 'text-blue-500'}`">
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>

      </div>
    </div>

    <h2 v-if="winner" class="text-6xl font-bold mb-8"> Player '{{ winner }}' wins!</h2>

    <button @click="resetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold 
    hover:bg-pink-600 duration-300">
      Reset Game
    </button>
  </main>
</template>
