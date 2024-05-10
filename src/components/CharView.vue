<template>
    <div class="container">
        <div class="status">
            <p>
                <span>{{ props.currentIndex }}</span
                >/{{ props.testLength }}
            </p>
        </div>
        <div class="char-container">
            <transition name="fade">
                <h1 :key="props.currentIndex" :class="{ correct: props.correctKeyPressed }">
                    {{ props.currentCharacter }}
                </h1>
            </transition>
        </div>
    </div>
</template>
<script setup>
const props = defineProps(['currentIndex', 'testLength', 'currentCharacter', 'correctKeyPressed'])
</script>

<style scoped>
.container {
    display: flex;
    /* background-color: aquamarine; */
    width: 100%;
}
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease-in-out;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}
h1 {
    font-size: 7em;
    color: var(--gruv-h1);
    text-shadow: 3px 3px 3px #00000017;
}
.status p {
    color: var(--gruv-h1);
    padding-top: 2.5em;
    position: absolute;
    font-size: 2em;
    /* background-color: blue; */
}
.status span {
    color: var(--gruv-accent);
}
.status {
    /* background-color: blue; */
    display: flex;
    /* height: 100px; */
}
.char-container {
    display: flex;
    align-items: center;
    justify-content: center;
    /* height: 60vh; */
    /* background-color: red; */
    width: 100%;
}
.correct {
    color: #689d6a;
}

.shake {
    animation: shake 0.5s;
}
.shake h1 {
    color: var(--gruv-wrong);
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
