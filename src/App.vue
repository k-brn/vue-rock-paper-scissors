<script setup>
import { ref, computed, onMounted } from "vue";

const wins = ref(0);
const draws = ref(0);
const losses = ref(0);

const choice = ref(null);
const computerChoice = ref(0);
const verdict = ref(0);

const outcomes = {
  rock: {
    rock: "draw",
    paper: "loss",
    scissors: "win",
  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "loss",
  },
  scissors: {
    rock: "loss",
    paper: "win",
    scissors: "draw",
  },
};

const winPercentage = computed(() => {
  const total = wins.value + draws.value + losses.value;
  return total ? (wins.value / total) * 100 : 0;
});

const play = (c) => {
  choice.value = c;

  const choices = ["rock", "paper", "scissors"];
  const random = Math.floor(Math.random() * 3);
  computerChoice.value = choice[random];

  const outcome = [c][computerChoice.value];

  if (outcome === "win") {
    wins.value++;
    verdict.value = "You win!";
  } else if (outcome === "loss") {
    losses.value++;
    verdict.value = "You lose!";
  } else {
    draws.value++;
    verdict.value = "It is a draw!";
  }

  SaveGame();
};

const SaveGame = () => {
  localStorage.setItem("wins", wins.value);
  localStorage.setItem("draws", draws.value);
  localStorage.setItem("losses", losses.value);
};

const LoadGame = () => {
  wins.value = localStorage.getItem("wins");
  draws.value = localStorage.getItem("draws");
  losses.value = localStorage.getItem("losses");
};

const ResetRound = () => {
  choice.value = null;
  computerChoice.value = null;
  verdict.value = null;
};

onMounted(() => {
  LoadGame();

  window.addEventListener("keypress", (e) => {
    if (e.key === "r") {
      ResetRound();
    }
  });
});
</script>

<template>
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header class="container mx-auto p-6">
      <h1 class="text-4xl font-bold">ROCK, PAPER, SCISSORS</h1>
    </header>
    <main class="container mx-auto p-6 flex-1">
      <div v-if="choice === null" class="flex items-center justify-center mx-6">
        <button
          @click="play('rock')"
          class="w-64 p-6 mx-6 transition-transform duration-300 transform hover:scale-110"
        >
          <img src="./assets/rock.png" alt="Rock" class="w-full" />
        </button>

        <button
          @click="play('paper')"
          class="w-64 p-6 mx-6 transition-transform duration-300 transform hover:scale-110"
        >
          <img src="./assets/paper.png" alt="Paper" class="w-full" />
        </button>

        <button
          @click="play('scissors')"
          class="w-64 p-6 mx-6 transition-transform duration-300 transform hover:scale-110"
        >
          <img src="./assets/scissors.png" alt="Scissors" class="w-full" />
        </button>
      </div>
    </main>
  </div>
</template>
