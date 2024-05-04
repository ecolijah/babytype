<template>
  <div class="bigboy">
    <TitleBarView/>
    <SettingsbarView @settingsChanged="handleSettingsChanged"/>
    <div v-if=!testFinished class="char-container">
      <transition name="fade">
        <h1 :key="currentIndex" :class="{ correct: correctKeyPressed }">{{ currentCharacter }}</h1>
      </transition>
    </div>
    <div v-if=testFinished class="statscreen">
      <h2>Mistakes: {{ numMissed }}</h2>
      <p>press <span>Tab</span> to begin next test</p>
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

const testTime = ref(null);
const testLength =  ref(26); //Defaults at 26 char of alphabet

const testFinished =  ref(false);
const numMissed = ref(0);


const handleKeyDown = (event) => {
  if (currentIndex.value === 0) {
    numMissed.value = 0;
  }
  if (event.key.toLowerCase() === currentCharacter.value) {
    correctKeyPressed.value = true;
    if (currentIndex.value===25) {
      testFinished.value = true;
    } 
    setTimeout(() => {

      currentIndex.value = (currentIndex.value + 1) % shuffledAlphabet.length;
      currentCharacter.value = shuffledAlphabet[currentIndex.value];
      correctKeyPressed.value = false;

    }, 50); // Delay the change after 500ms to see the green color
  } else {
    correctKeyPressed.value = false;
    numMissed.value +=1;
    // Add shake effect
    document.body.classList.add('shake');
    setTimeout(() => {
      document.body.classList.remove('shake');
    }, 50);
  }

  if (event.key === "Tab") {
    testFinished.value = false;
  }
};

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeyDown);
});

const handleSettingsChanged = (selectedSettings) => {
  console.log("handling settings changed.")
  // Check selected settings and render the appropriate list
  //first index settings
  const alphabetMode = selectedSettings[0].index === 2;
  const randomMode = selectedSettings[0].index === 0;

  // const weakMode = selectedSettings[0].index === 1;

  //2nd index settings
  const timeMode = selectedSettings[1].index === 0;
  const lettersMode = selectedSettings[1].index === 1;

  //3rd index settings
  const tenUnits = selectedSettings[2].index === 0;
  const twentyFiveUnits = selectedSettings[2].index === 1;
  const fiftyUnits = selectedSettings[2].index === 2;
  const hundredUnits = selectedSettings[2].index === 3;


  if (alphabetMode) {
    // Render alphabet list
    shuffledAlphabet = originalAlphabet.split(''); // Reset shuffled alphabet to original alphabet
    currentIndex.value = 0; // Reset index
    currentCharacter.value = shuffledAlphabet[currentIndex.value];
  }
  if (randomMode) {
    // Shuffle the alphabet
    shuffledAlphabet = shuffleArray(shuffledAlphabet);
    // Start from the beginning of the shuffled alphabet
    currentIndex.value = 0;
    currentCharacter.value = shuffledAlphabet[currentIndex.value];
  }

  //This is where we will handle the time/letters and value buttons
  if (tenUnits){
    testTime.value = 10; //Ten seconds
    testLength.value = 10;
  } 
  if (twentyFiveUnits) {
    testTime.value = 25;
    testLength.value = 25;
    
  }
  if (fiftyUnits) {
    testTime.value = 50;
    testLength.value = 50;
    
  }
  if (hundredUnits) {
    testTime.value = 100;
    testLength.value = 100;
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

.statscreen {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  color: var(--gruv-h1);
  height: 70vh;
  /* background-color: #689D6A; */
}

.statscreen span {
  color:  var(--gruv-background);
  font-weight: 700;
  background-color: var(--gruv-h1);
  padding-left: 3px;
  padding-right: 3px;
  border-radius: 3px;
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
