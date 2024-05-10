<template>
    <div class="container">
        <div class="titlebar">
            <div class="blockx">
                <img src="/src/components/icons/icons8-babys-room-64.png" />
                <RouterLink to="/" style="text-decoration: none"><h1>babytype</h1></RouterLink>
            </div>
        </div>
        <div class="settings">
            <div class="settingsbar">
                <div class="block">
                    <button :class="{ active: isActive(0, 0) }" @click="selectButton(0, 0)">
                        <h3>? random</h3>
                    </button>
                    <button :class="{ active: isActive(2, 0) }" @click="selectButton(2, 0)">
                        <h3>alphabet</h3>
                    </button>
                </div>
            </div>
            <div class="settingsbar">
                <div class="block">
                    <button :class="{ active: isActive(0, 2) }" @click="selectButton(0, 2)">
                        <h3>10</h3>
                    </button>
                    <button :class="{ active: isActive(1, 2) }" @click="selectButton(1, 2)">
                        <h3>25</h3>
                    </button>
                    <button :class="{ active: isActive(2, 2) }" @click="selectButton(2, 2)">
                        <h3>50</h3>
                    </button>
                    <button :class="{ active: isActive(3, 2) }" @click="selectButton(3, 2)">
                        <h3>100</h3>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['settingsChanged'])

var selectedButtons = ref([
    { index: 2, blockIndex: 0 }, // Default: random
    { index: null, blockIndex: null }, // Default: Letters
    { index: null, blockIndex: null } // Default: 25
])

const selectButton = (index, blockIndex) => {
    // If alphabet button is already selected in block index 0, prevent selections in other blocks
    if (selectedButtons.value[0].index === 2 && blockIndex !== 0) {
        return // Do nothing if alphabet button is already selected
    }

    if (index === 2 && blockIndex === 0) {
        // Update selectedButtons to have only the alphabet selected
        selectedButtons.value = [
            { index: 2, blockIndex: 0 }, // Default: Alphabet
            { index: null, blockIndex: null }, // Default: Letters
            { index: null, blockIndex: null }
        ]
    } else {
        // Update the selected button as usual
        selectedButtons.value[blockIndex] = { index, blockIndex }

        // Populate null values with defaults if necessary
        selectedButtons.value.forEach((button, i) => {
            if (button.index === null) {
                switch (i) {
                    case 1:
                        selectedButtons.value[i] = { index: 1, blockIndex: i } // Default: Letters
                        break
                    case 2:
                        selectedButtons.value[i] = { index: 1, blockIndex: i } // Default: 25
                        break
                    // Add additional cases for other default values if needed
                    default:
                        break
                }
            }
        })
    }

    // Emit event with selected settings
    emit('settingsChanged', selectedButtons.value)
}

const isActive = (index, blockIndex) => {
    const selectedButton = selectedButtons.value[blockIndex]
    return (
        selectedButton && selectedButton.index === index && selectedButton.blockIndex === blockIndex
    )
}
</script>

<style scoped>
.settings {
    display: flex;
    width: 70%;
    justify-content: flex-end;
}
.container {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    width: 100%;
    flex-wrap: wrap;
}
.settingsbar {
    display: flex;
    flex-direction: row;
    background-color: var(--gruv-back-layer);
    height: 40px;
    /* width: 220px; */
    align-items: center;
    border-radius: 10px;
    margin-top: 40px;
    box-shadow: 4px 4px 10px 4px #0000000e;
    margin-left: 10px;

    /* width: 50% */
}

.block {
    display: flex;
    padding-left: 10px;
    padding-right: 10px;
}

button {
    background: none;
    border: none;
    cursor: pointer;
    position: relative;
}

h3 {
    font-size: 1em;
    color: var(--gruv-h1);
    padding-left: 8px;
    padding-right: 8px;
}

img {
    max-height: 40px;
    padding-right: 2px;
}
.blockx {
    display: flex;
    align-items: center;
}
.titlebar {
    display: flex;
}
h1 {
    font-weight: 500;
    color: var(--gruv-titles);
    text-shadow: 3px 3px 3px #00000017;
}

button:hover h3 {
    color: var(--gruv-titles);
    transition: color 0.2s ease-in-out;
}

button.active h3 {
    color: var(--gruv-accent);
    text-shadow: 1px 1px 3px #00000017;
}
</style>
