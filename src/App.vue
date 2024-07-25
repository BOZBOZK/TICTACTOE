<script setup lang="ts">
import { ref, computed } from 'vue'

// a is defined to determine the turn. If a=1 X plays. else, O.
let a = 1
let Xscore = 0
let Oscore = 0


// Buttons in board
interface Button {
  id: number;
  label: string | null;
}

const buttons = ref<Button[]>([
  { id: 1, label: null },
  { id: 2, label: null },
  { id: 3, label: null },
  { id: 4, label: null },
  { id: 5, label: null },
  { id: 6, label: null },
  { id: 7, label: null },
  { id: 8, label: null },
  { id: 9, label: null },
]);

const XclickedNumbers = ref<string[]>([]);
const OclickedNumbers = ref<string[]>([]);
const Xturn = ref(false);
const Oturn = ref(false);
//const playerName = ref('');
const showNameInput = ref(false);

const resetGame = () => {
  resetButtons();
  Xscore = 0;
  Oscore = 0;
  Xturn.value = false;
  Oturn.value = false;
}

//changes color of circles to show turn.
const turn = () => {
  if (a === 3) {
    Oturn.value = true;
    Xturn.value = false;
  } else {
    Xturn.value = true;
    Oturn.value = false;
  }
}

const resetButtons = () => {
  buttons.value.forEach(button => {
    button.label = null;
  });
  XclickedNumbers.value = [];
  OclickedNumbers.value = [];
}

const checker = (winningPattern: string[][], clickedNumbers: string[]): boolean => {
  return winningPattern.some(pattern => pattern.every(num => clickedNumbers.includes(num)));
};

const winngAlert = (winner: string) => {
  alert(`${winner} Win`);
}

const drawAlert = () => {
  alert('Draw!');
}
// i know winner could me determined only by checking checker. i just wanted to use compured XD
const winner = computed(() => {
  if (checker(winningPattern, XclickedNumbers.value)) {
    return 'X';
  } else if (checker(winningPattern, OclickedNumbers.value)) {
    return 'O';
  }
  return null;
});

const toggleLabel = (button: Button) => {
  const currentTurn = computed(() => (a === 1 ? 'X' : 'O'));
  
  turn();
  if (button.label !== 'X' && button.label !== 'O') {
    if (currentTurn.value === 'X'){
    XclickedNumbers.value.push(button.id.toString());
    button.label = 'X';
    a = 3;
    if (winner.value === 'X') {
      winngAlert('X');
      Xscore += 1;
      resetButtons();
    }
  } else {
    OclickedNumbers.value.push(button.id.toString());
    button.label = 'O';
    a = 1;
    if (winner.value === 'O') {
      winngAlert('O');
      Oscore += 1;
      resetButtons();
    }
  }
  if (XclickedNumbers.value.length + OclickedNumbers.value.length === 8) {
    drawAlert();
    resetButtons();
  }
}
  turn();
}

const winningPattern = [
  ['1', '2', '3'],
  ['4', '5', '6'],
  ['7', '8', '9'],
  ['1', '5', '9'],
  ['3', '5', '7'],
  ['1', '4', '7'],
  ['2', '5', '8'],
  ['3', '6', '9']
];



/*
const toggleNameInput = () => {
  showNameInput.value = !showNameInput.value;
}
*/
</script>

<template>
  <main class="flex justify-center items-center min-h-screen bg-blue-500">
    <!-- <audio scr="Vue/TicTacToe/public/Yellow Magic Orchestra - Rydeen (2018 Bob Ludwig Remastering.mp3" controls ></audio> -->
    <div id="main" class="text-center" >
      <div id="game-name-and-turn" class="flex justify-between items-center w-full">
        <div id="Xturn" :class="['w-12 h-12 rounded-full border-4 border-black', Xturn ? 'bg-red-600' : 'bg-blue-300']"></div>
        <h1 id="name" class="text-4xl font-bold">TicTacToe!</h1>
        <div id="Oturn" :class="['w-12 h-12 rounded-full border-4 border-black', Oturn ? 'bg-blue-700' : 'bg-blue-300']"></div>
      </div>
      <div id="players" class="flex justify-between items-center w-full">
        <div id="Xplayer" class="flex flex-col items-center bg-red-600 p-4 rounded-lg">
          <!-- <div class="cursor-pointer" @click="toggleNameInput">
            add name
            <div v-if="showNameInput" class="mt-2">
              <input v-model="playerName" type="text" class="p-2 rounded border">
            </div>
          </div> -->
          <h1 class="text-2xl font-bold">X</h1>
          <h3 class="text-xl">Score: {{ Xscore }}</h3>
        </div>
        <div id="Oplayer" class="flex flex-col items-center bg-blue-700 p-4 rounded-lg text-white">
          <h1 class="text-2xl font-bold">O</h1>
          <h3 class="text-xl">Score: {{ Oscore }}</h3>
        </div>
      </div>
      <div id="board" class="grid grid-cols-3 gap-2 my-8">
        <button v-for="button in buttons" :key="button.id" class="w-24 h-24 bg-blue-700 text-white text-2xl font-bold hover:bg-blue-900" @click="toggleLabel(button)">
          {{ button.label }}
        </button>
      </div>

      <button id="reset-button" class="bg-gray-700 text-white px-4 py-2 rounded hover:bg-black" @click="resetGame"><b>reset game</b></button>
    </div>
  </main>
</template>