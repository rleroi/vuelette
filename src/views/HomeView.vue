<script setup lang="js">
import { ref } from 'vue';

let money = ref(100);
let result = ref(-1);
const currentBets = ref([]);
let selectedAmount = ref(1);

function bet(what) {
  money.value -= selectedAmount.value;
  currentBets.value.push(what);
}

function getRandomNumber() {
  return Math.floor(Math.random() * 36);
}

function isRed(i) {
  return (i % 2) === 0;
}

function start() {
  result.value = getRandomNumber();
  console.log(result);

  let winAmount = 0;

  const numbersBetsWon = currentBets.value.filter((i) => i === result.value).length;
  winAmount += (numbersBetsWon * 36);

  // black/red
  if (isRed(result.value) ) {
    const redBetsWon = currentBets.value.filter((i) => i === 'RED').length;
    winAmount += (redBetsWon * 2)
  } else {
    const blackBetsWon = currentBets.value.filter((i) => i === 'BLACK').length;
    winAmount += (blackBetsWon * 2)
  }

  if (winAmount) {
    window.alert(`RESULT: ${result.value}
You won ${winAmount}`);
    money.value += winAmount;
  } else {
    window.alert(`RESULT: ${result.value}
You lost.`);
  }

  currentBets.value = [];
}

</script>

<template>
  <main class="w-full h-full flex flex-col items center">

    <div class="grid grid-cols-2">
      <div class="flex flex-col text-white text-center">
        <div class="border border-white bg-green" role="button" @click="bet(0)" :class="{'bg-gray': currentBets.includes(0)}">0</div>
        <div class="grid grid-cols-12 grid-rows-3 grid-flow-col">
          <div role="button" class="border border-white bg-black text-2xl py-2 px-6" :class="{'bg-gray': currentBets.includes(i), 'bg-red': isRed(i)}" v-for="i in 36" :key="i" @click="bet(i)">{{ i }}</div>
        </div>
        <div class="grid grid-cols-2">
          <div class="border border-white bg-red" role="button" @click="bet('RED')" :class="{'bg-gray': currentBets.includes('RED')}">RED</div>
          <div class="border border-white bg-black" role="button" @click="bet('BLACK')" :class="{'bg-gray': currentBets.includes('BLACK')}">BLACK</div>
        </div>
      </div>
    </div>

    <div class="w-full mt-auto">
      <div class="flex">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
          class="w-6 h-6">
          <path stroke-linecap="round" stroke-linejoin="round"
            d="M14.25 7.756a4.5 4.5 0 1 0 0 8.488M7.5 10.5h5.25m-5.25 3h5.25M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
        </svg>
        <span class="ml-1">{{ money }}</span>
      </div>
      <div><button @click="start">START</button></div>
    </div>
  </main>
</template>
