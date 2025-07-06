<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import MemoryCard from './components/MemoryCard.vue'

const shuffleCards = (array) => {
  const result = [...array];
  for (let i = result.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [result[i], result[j]] = [result[j], result[i]];
  }
  return result;
};

const symbols = [
  {
    name: "girl1",
    image: new URL('./assets/00-featured-maki-oze-muscley-girl-anime-screenshot.jpg', import.meta.url).href
  },
  {
    name: "girl2",
    image: new URL('./assets/978.png', import.meta.url).href,
  },
  {
    name: "girl3",
    image: new URL('./assets/street-fighter-x-tekken-nina-williams.jpg', import.meta.url).href,
  },
  {
    name: "girl4",
    image: new URL('./assets/Без названия.jpg', import.meta.url).href,
  },
  {
    name: "girl5",
    image: new URL('./assets/alisa-boskonovich-tekken-6-kiborg-devushka-art-lico-vzglyad-volosy.jpg', import.meta.url).href,
  },
  {
    name: "girl6",
    image: new URL('./assets/anime-anime-girl-cute-girl.jpg', import.meta.url).href,
  },
  {
    name: "girl7",
    image: new URL('./assets/artworks-33PAy6LsFSR7zZvz-Vsw4GQ-t500x500.jpg', import.meta.url).href,
  },
  {
    name: "girl8",
    image: new URL('./assets/avatars-Zb6FhWSzovp4iWWr-61CkuA-t500x500.jpg', import.meta.url).href,
  },

];

const cards = ref([]);
const moves = ref(0);
// const index = ref(0);
const totalPairs = symbols.length * 2;
const matchedCards = ref(new Set<number>());
const openedCards = ref(new Set<number>());
const matches = computed(() => matchedCards.value.size);
const isGameWon = computed(() => matches.value === totalPairs)

function resetGame() {
  moves.value = 0;
  openedCards.value.clear();
  matchedCards.value.clear();

  cards.value = shuffleCards([...symbols, ...symbols]);
}

function openCard(index) {
  moves.value += 1;
  openedCards.value.add(index);
};

function getStatus(index) {
  if (openedCards.value.has(index)) {
    return "opened";
  }
  if (matchedCards.value.has(index)) {
    return "matched";
  }
  return "closed"
}

const CLOSE_TIMEOUT = 1000
const hasTwoCardsOpened = computed(() => openedCards.value.size === 2);
watch(hasTwoCardsOpened, (areTwoCardsOpened) => {
  if (!areTwoCardsOpened) {
    return;
  }

  setTimeout(() => {
    openedCards.value.clear()
  }, CLOSE_TIMEOUT);

  const [firstIndex, secondIndex] = [...openedCards.value.values()];
  if (cards.value[firstIndex].name === cards.value[secondIndex].name) {
    matchedCards.value.add(firstIndex)
    matchedCards.value.add(secondIndex)

  }
});


resetGame();



</script>

<template>
  <div id="app">
    <h1>Игра найди пару</h1>

    <div class="game-info">
      <div>Ходов: {{ moves }}</div>
      <div>Пары: {{ matches }} / {{ totalPairs }}</div>
    </div>

    <div class="board">
      <MemoryCard v-for="(card, index) in cards" :disabled="hasTwoCardsOpened" :key="index" :status="getStatus(index)"
        :image="card.image" @click="openCard(index)" />

    </div>

    <button @click="resetGame">Новая игра</button>

    <div v-if="isGameWon" class="win-message">
      Поздравляем вы выйграли за {{ moves }} ходов!
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Arial", sans-serif;
  background-color: #f4f7f9;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 20px;
}

#app {
  width: 100%;
  max-width: 800px;
  text-align: center;
}

h1 {
  color: #2c3e50;
  margin-bottom: 20px;
}

.game-info {
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  padding: 0 10px;
}

.game-info div {
  font-size: 1.2rem;
  font-weight: bold;
  color: #34495e;
}

.board {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 15px;
  margin: 0 auto;
}



button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #2980b9;
}

.win-message {
  margin-top: 20px;
  font-size: 1.5rem;
  color: #27ae60;
  font-weight: bold;
}

@media (max-width: 600px) {
  .board {
    grid-template-columns: repeat(3, 1fr);
  }

  .card {
    height: 100px;
  }
}
</style>
