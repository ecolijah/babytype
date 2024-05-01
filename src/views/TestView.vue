<template>
  <div class="bigboy">
    <TitleBarView/>
    <SettingsbarView @settingsChanged="handleSettingsChanged"/>
    <div class="char-container">
      <transition name="fade">
        <h1 :key="currentIndex" :class="{ correct: correctKeyPressed }">{{ currentCharacter }}</h1>
      </transition>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import TitleBarView from "../components/TitlebarView.vue"
import SettingsbarView from '../components/SettingsbarView.vue';

const originalAlphabet = "abcdefghijklmnopqrstuvwxyz";
let shuffledAlphabet = originalAlphabet.split(''); // Create a copy of the original alphabet array
const currentIndex = ref(0);
const currentCharacter = ref(shuffledAlphabet[currentIndex.value]);
const correctKeyPressed = ref(false);

const handleKeyDown = (event) => {
  if (event.key.toLowerCase() === currentCharacter.value) {
    correctKeyPressed.value = true;
    setTimeout(() => {
      currentIndex.value = (currentIndex.value + 1) % shuffledAlphabet.length;
      currentCharacter.value = shuffledAlphabet[currentIndex.value];
      correctKeyPressed.value = false;
    }, 50); // Delay the change after 500ms to see the green color
  } else {
    correctKeyPressed.value = false;
    // Add shake effect
    document.body.classList.add('shake');
    setTimeout(() => {
      document.body.classList.remove('shake');
    }, 50);
  }
};

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeyDown);
});

const handleSettingsChanged = (selectedSettings) => {
  // Check selected settings and render the appropriate list
  const alphabetMode = selectedSettings[0].index === 2;
  if (alphabetMode) {
    // Render alphabet list
    shuffledAlphabet = originalAlphabet.split(''); // Reset shuffled alphabet to original alphabet
    currentIndex.value = 0; // Reset index
    currentCharacter.value = shuffledAlphabet[currentIndex.value];
  } else {
    // Shuffle the alphabet
    shuffledAlphabet = shuffleArray(shuffledAlphabet);
    // Start from the beginning of the shuffled alphabet
    currentIndex.value = 0;
    currentCharacter.value = shuffledAlphabet[currentIndex.value];
  }
};

// Function to shuffle array elements
const shuffleArray = (array) => {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
};
</script>


<style scoped>
.bigboy {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.char-container {
  display: flex;
  align-items: center;
  height: 70vh;
}

h1 {
  font-size: 5em;
  color: var(--gruv-h1);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease-in-out;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.correct {
  color: #689D6A;
}

.shake {
  animation: shake 0.5s;
}

@keyframes shake {
  0% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  50% { transform: translateX(5px); }
  75% { transform: translateX(-5px); }
  100% { transform: translateX(0); }
}
</style>
