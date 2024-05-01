<template>
    <div class="settingsbar">
        <div class="block">
            <button :class="{ active: isActive(0, 0) }" @click="selectButton(0, 0)"><h3>? random</h3></button>
            <button :class="{ active: isActive(1, 0) }" @click="selectButton(1, 0)"><h3>x weakness</h3></button>
            <button :class="{ active: isActive(2, 0) }" @click="selectButton(2, 0)"><h3>alphabet</h3></button>
        </div>

        <div class="block">
            <button :class="{ active: isActive(0, 1) }" @click="selectButton(0, 1)"><h3>time</h3></button>
            <button :class="{ active: isActive(1, 1) }" @click="selectButton(1, 1)"><h3>letters</h3></button>
        </div>

        <div class="block">
            <button :class="{ active: isActive(0, 2) }" @click="selectButton(0, 2)"><h3>10</h3></button>
            <button :class="{ active: isActive(1, 2) }" @click="selectButton(1, 2)"><h3>25</h3></button>
            <button :class="{ active: isActive(2, 2) }" @click="selectButton(2, 2)"><h3>50</h3></button>
            <button :class="{ active: isActive(3, 2) }" @click="selectButton(3, 2)"><h3>100</h3></button>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const selectedButtons = ref([
    { index: 2, blockIndex: 0 }, // Default: Alphabet
    { index: 1, blockIndex: 1 }, // Default: Letters
    { index: 1, blockIndex: 2 }  // Default: 25
]);

const selectButton = (index, blockIndex) => {
    selectedButtons.value[blockIndex] = { index, blockIndex };
};

const isActive = (index, blockIndex) => {
    const selectedButton = selectedButtons.value[blockIndex];
    return selectedButton && selectedButton.index === index && selectedButton.blockIndex === blockIndex;
};
</script>

<style scoped>
.settingsbar {
    display: flex;
    flex-direction: row;
    background-color: var(--gruv-back-layer);
    height: 40px;
    align-items: center;
    border-radius: 10px;
    margin-top: 40px;
}

.block {
    display: flex;
    padding-left: 20px;
    padding-right: 20px;
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

button:hover h3 {
    color: var(--gruv-titles);
    transition: color 0.2s ease-in-out;
}

button.active h3 {
    color: var(--gruv-accent);
}
</style>
