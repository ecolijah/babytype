<template>
  <div class="bigboy">
    <TitleBarView/>
    <SettingsbarView/>
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

const alphabet = "abcdefghijklmnopqrstuvwxyz";
const currentIndex = ref(0);
const currentCharacter = ref(alphabet[currentIndex.value]);
const correctKeyPressed = ref(false);

const handleKeyDown = (event) => {
  if (event.key.toLowerCase() === currentCharacter.value) {
    correctKeyPressed.value = true;
    setTimeout(() => {
      currentIndex.value = (currentIndex.value + 1) % alphabet.length;
      currentCharacter.value = alphabet[currentIndex.value];
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
</script>

<style scoped>
.bigboy {
  display: flex;
  flex-direction: column;
  /* background-color: red; */
  align-items: center;
  /* justify-content: center; */
  /* height: 50vh; */
}
.char-container {
  display: flex;
  /* background-color: blueviolet; */
  align-items: center;
  height: 70vh;
}

h1 {
  font-size: 5em;
  color: var(--gruv-h1);
  /* background-color: black; */
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
