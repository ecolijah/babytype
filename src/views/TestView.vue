<template>
    <div class="bigboy">
        <SettingsbarView
            :weakness="weakness"
            :num-tests="numTests"
            @settingsChanged="handleSettingsChanged"
        />
        <div class="test-container">
            <transition name="fade" mode="out-in">
                <component
                    :is="activeComponent"
                    :accuracy="acc"
                    :missedmap="missedMap"
                    :cpm="cpm"
                    :testLength="testLength"
                    :currentIndex="currentIndex"
                    :currentCharacter="currentCharacter"
                    :correctKeyPressed="correctKeyPressed"
                ></component>
            </transition>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, shallowRef } from 'vue'
import SettingsbarView from '../components/SettingsbarView.vue'
import ScoreView from '../components/ScoreView.vue'
import CharView from '../components/CharView.vue'

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
const weakMode = ref(false)

//time variables
let startTime = Date.now()
let endTime = Date.now()

const timeElapsed = ref(null)

const cpm = ref(null)
const numTests = ref(0)
const activeComponent = shallowRef(CharView) //default

//weakness in browserr mem
const weakness = new Map()

const handleKeyDown = (event) => {
    if (event.key.toLowerCase() === currentCharacter.value && testFinished.value == false) {
        correctKeyPressed.value = true
        if (currentIndex.value === 0) {
            //start timer
            startTime = Date.now()
        }
        if (currentIndex.value === shuffledAlphabet.length - 1) {
            acc.value = Math.round(
                ((shuffledAlphabet.length - numMissed.value) / shuffledAlphabet.length) * 100
            )
            //stop timer
            endTime = Date.now()

            timeElapsed.value = endTime - startTime
            cpm.value = Math.floor((shuffledAlphabet.length / (timeElapsed.value / 1000)) * 60)
            //switch views
            testFinished.value = true
            numTests.value += 1
            activeComponent.value = ScoreView
            console.log(weakness)
        }
        setTimeout(() => {
            currentIndex.value = (currentIndex.value + 1) % shuffledAlphabet.length
            currentCharacter.value = shuffledAlphabet[currentIndex.value]
            correctKeyPressed.value = false
        }, 50) // Delay the change after 500ms to see the green color
    } else if (testFinished.value == false && event.key !== 'Tab') {
        correctKeyPressed.value = false
        numMissed.value += 1
        //add to hashmap
        if (missedMap.has(currentCharacter.value)) {
            var tmp = missedMap.get(currentCharacter.value)
            missedMap.set(currentCharacter.value, tmp + 1)
        } else {
            missedMap.set(currentCharacter.value, 1)
        }

        if (weakness.has(currentCharacter.value)) {
            var tmp2 = weakness.get(currentCharacter.value)
            weakness.set(currentCharacter.value, tmp2 + 1)
        } else {
            weakness.set(currentCharacter.value, 1)
        }

        // Add shake effect
        const charContainer = document.querySelector('.char-container')
        charContainer.classList.add('shake')
        setTimeout(() => {
            charContainer.classList.remove('shake')
        }, 500)
    }

    if (event.key === 'Tab') {
        event.preventDefault()
        testFinished.value = false
        currentIndex.value = 0
        numMissed.value = 0
        missedMap.clear()
        activeComponent.value = CharView

        if (alphabetMode.value) {
            // Render alphabet list
            shuffledAlphabet = originalAlphabet.split('') // Reset shuffled alphabet to original alphabet
        }
        if (randomMode.value) {
            // Shuffle the alphabet

            shuffledAlphabet = shuffleArray(originalAlphabet.split(''), testLength.value)
            // Start from the beginning of the shuffled alphabet
            //use units to lengthen alphabet if necessary
        }
        if (weakMode.value) {
            //weak shuffle logic here
            var tmpList = mapToList(weakness)
            shuffledAlphabet = shuffleArray(tmpList, testLength.value)
        }
        currentIndex.value = 0
        currentCharacter.value = shuffledAlphabet[currentIndex.value]
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
    weakMode.value = selectedSettings[0].index === 1
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
        shuffledAlphabet = shuffleArray(originalAlphabet.split(''), testLength.value)
    }
    if (weakMode.value) {
        //weak shuffle logic here
        var tmpList = mapToList(weakness)
        shuffledAlphabet = shuffleArray(tmpList, testLength.value)
    }

    currentIndex.value = 0
    currentCharacter.value = shuffledAlphabet[currentIndex.value]
    testFinished.value = false
    numMissed.value = 0
    missedMap.clear()
    activeComponent.value = CharView
}

function mapToList(map) {
    const result = []
    map.forEach((value, key) => {
        for (let i = 0; i < value; i++) {
            result.push(key)
        }
    })
    return result
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
    width: 100%;
    display: flex;
    height: 80vh;
    padding: 10px;
}
.status p {
    color: var(--gruv-h1);
    padding-top: 2.5em;
    position: absolute;
    font-size: 2em;
}
.status span {
    color: var(--gruv-accent);
}
.status {
    display: flex;
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
    width: 100%;
}

.statscreen {
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--gruv-h1);
    height: 70vh;
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
    text-shadow: 3px 3px 3px #00000017;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}
</style>
