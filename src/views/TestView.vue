<template>
    <div class="bigboy">
        <TitleBarView />
        <SettingsbarView @settingsChanged="handleSettingsChanged" />

        <div class="test-container">
            <div v-if="!testFinished" class="status">
                <p>
                    <span>{{ currentIndex + 1 }}</span
                    >/{{ testLength }}
                </p>
            </div>

            <div v-if="!testFinished" class="char-container">
                <transition name="fade">
                    <h1 :key="currentIndex" :class="{ correct: correctKeyPressed }">
                        {{ currentCharacter }}
                    </h1>
                </transition>
            </div>

            <div style="width: 100%" v-if="testFinished">
                <ScoreView :accuracy="acc" :missedmap="missedMap" />
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import TitleBarView from '../components/TitlebarView.vue'
import SettingsbarView from '../components/SettingsbarView.vue'
import ScoreView from '../components/ScoreView.vue'

const originalAlphabet = 'abcdefghijklmnopqrstuvwxyz'
let shuffledAlphabet = originalAlphabet.split('') // Create a copy of the original alphabet array
const currentIndex = ref(0)
const currentCharacter = ref(shuffledAlphabet[currentIndex.value])
const correctKeyPressed = ref(false)

const testLength = ref(26) //Defaults at 26 char of alphabet

const testFinished = ref(false)
const numMissed = ref(0)
const acc = ref(0)

const missedMap = new Map()

const randomMode = ref(false)
const alphabetMode = ref(true)

const handleKeyDown = (event) => {
    if (event.key.toLowerCase() === currentCharacter.value && testFinished.value == false) {
        correctKeyPressed.value = true
        if (currentIndex.value === 0) {
            //start timer
        }
        if (currentIndex.value === shuffledAlphabet.length - 1) {
            acc.value = Math.round(
                ((shuffledAlphabet.length - numMissed.value) / shuffledAlphabet.length) * 100
            )
            testFinished.value = true
            //stop timer
        }
        setTimeout(() => {
            currentIndex.value = (currentIndex.value + 1) % shuffledAlphabet.length
            currentCharacter.value = shuffledAlphabet[currentIndex.value]
            correctKeyPressed.value = false
        }, 50) // Delay the change after 500ms to see the green color
    } else if (testFinished.value == false) {
        correctKeyPressed.value = false
        numMissed.value += 1
        //add to hashmap
        if (missedMap.has(currentCharacter.value)) {
            var tmp = missedMap.get(currentCharacter.value)
            missedMap.set(currentCharacter.value, tmp + 1)
        } else {
            missedMap.set(currentCharacter.value, 1)
        }

        // Add shake effect
        document.body.classList.add('shake')
        setTimeout(() => {
            document.body.classList.remove('shake')
        }, 50)
    }

    if (event.key === 'Tab') {
        testFinished.value = false
        currentIndex.value = 0
        numMissed.value = 0
        missedMap.clear()

        if (alphabetMode.value) {
            // Render alphabet list
            shuffledAlphabet = originalAlphabet.split('') // Reset shuffled alphabet to original alphabet
            currentIndex.value = 0 // Reset index
            currentCharacter.value = shuffledAlphabet[currentIndex.value]
        }
        if (randomMode.value) {
            // Shuffle the alphabet
            shuffledAlphabet = shuffleArray(shuffledAlphabet, testLength.value)
            // Start from the beginning of the shuffled alphabet
            //use units to lengthen alphabet if necessary
            currentIndex.value = 0
            currentCharacter.value = shuffledAlphabet[currentIndex.value]
        }
    }
}

onMounted(() => {
    window.addEventListener('keydown', handleKeyDown)
})

onBeforeUnmount(() => {
    window.removeEventListener('keydown', handleKeyDown)
})

const handleSettingsChanged = (selectedSettings) => {
    console.log('handling settings changed.')

    alphabetMode.value = selectedSettings[0].index === 2
    randomMode.value = selectedSettings[0].index === 0

    const unitsMap = new Map([
        [0, 10],
        [1, 25],
        [2, 50],
        [3, 100]
    ])

    testLength.value = unitsMap.get(selectedSettings[2].index) || testLength.value

    if (alphabetMode.value) {
        shuffledAlphabet = originalAlphabet.split('')
        testLength.value = 26
    }
    if (randomMode.value) {
        shuffledAlphabet = shuffleArray(shuffledAlphabet, testLength.value)
    }

    currentIndex.value = 0
    currentCharacter.value = shuffledAlphabet[currentIndex.value]
}

// Function to shuffle array elements
const shuffleArray = (array, desiredLength) => {
    const shuffled = array.slice() // Make a copy of the original array to shuffle
    const originalLength = array.length

    // Fisher-Yates shuffle algorithm to shuffle the array
    for (let i = originalLength - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1))
        ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
    }

    // Repeat the shuffled array to fill the desired length
    const repeatedShuffled = []
    while (repeatedShuffled.length < desiredLength) {
        repeatedShuffled.push(...shuffled)
    }

    // Return the portion of the repeated array matching the desired length
    return repeatedShuffled.slice(0, desiredLength)
}
</script>

<style scoped>
.test-container {
    /* background-color: aliceblue;  */
    width: 80%;
    display: flex;
    height: 70vh;
    padding: 10px;
}
.status p {
    color: var(--gruv-h1);
    padding: 1em;
    position: absolute;
    /* background-color: blue; */
}
.status span {
    color: var(--gruv-accent);
}
.status {
    background-color: blue;
    display: flex;
    height: 100px;
}

.bigboy {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.char-container {
    display: flex;
    align-items: center;
    justify-content: center;
    /* height: 60vh; */
    /* background-color: red; */
    width: 100%;
}

.statscreen {
    display: flex;
    align-items: center;
    /* flex-direction: column; */
    justify-content: center;
    color: var(--gruv-h1);
    height: 70vh;
    /* background-color: #689D6A; */
}

.statscreen span {
    color: var(--gruv-background);
    font-weight: 700;
    background-color: var(--gruv-h1);
    padding-left: 3px;
    padding-right: 3px;
    border-radius: 3px;
}

h1 {
    font-size: 7em;
    color: var(--gruv-h1);
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease-in-out;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}

.correct {
    color: #689d6a;
}

.shake {
    animation: shake 0.5s;
}

@keyframes shake {
    0% {
        transform: translateX(0);
    }
    25% {
        transform: translateX(-5px);
    }
    50% {
        transform: translateX(5px);
    }
    75% {
        transform: translateX(-5px);
    }
    100% {
        transform: translateX(0);
    }
}
</style>
