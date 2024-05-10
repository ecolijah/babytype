<template>
    <div class="stats-container">
        <div class="stats">
            <div class="block">
                <h2>acc</h2>
                <h1>{{ accuracy }}%</h1>
            </div>

            <div class="block">
                <h2 v-if="!perfect">miss</h2>
                <!-- <h1> {{ missedmap }}</h1> -->
                <div class="missed">
                    <!-- <h1 v-for="item in missedmap" :key="item">{{ item[0]}},</h1> -->
                    <h1 v-for="(item, index) in missedmap" :key="item">
                        {{ item[0] }}<span v-if="index !== missedmap.size - 1">,</span>
                    </h1>
                </div>
            </div>

            <div class="block">
                <h2>cpm</h2>
                <h1>{{ cpm }}</h1>
            </div>
        </div>
        <div class="instructions">
            <p>press <span>Tab</span> to begin next test</p>
        </div>
    </div>
</template>
<script setup>
import { ref } from 'vue'
const props = defineProps(['accuracy', 'missedmap', 'cpm'])
const perfect = ref(props.missedmap.size === 0)
console.log(props.missedmap)
</script>

<style scoped>
.missed {
    display: flex;
    flex-direction: row;
}
.missed span {
    color: var(--gruv-h1);
}
.missed h1 {
    color: var(--gruv-wrong);
}
.stats-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 70vh;
    width: 100%;
    /* background-color: aliceblue; */
}
.stats {
    display: flex;
    align-items: flex-start;
    flex-direction: row;
    justify-content: center;
    color: var(--gruv-h1);
    /* width: 100%; */
    background-color: var(--gruv-back-layer);
    border-radius: 10px;
    box-shadow: 4px 4px 10px 4px #0000000e;
}
.instructions {
    color: var(--gruv-h1);
    padding-top: 40px;
}

.instructions span {
    color: var(--gruv-background);
    font-weight: 700;
    background-color: var(--gruv-h1);
    padding-left: 3px;
    padding-right: 3px;
    border-radius: 3px;
}

h1 {
    color: var(--gruv-accent);
    font-size: 3.5em;
    font-weight: 500;
    text-shadow: 3px 3px 3px #00000017;

    /* background-color: brown; */
}
h2 {
    font-weight: 400;
    font-size: 2.5em;
    margin: 0;
}
.block {
    padding: 25px;
}
</style>
