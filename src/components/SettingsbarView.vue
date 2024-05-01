<template>
    <div class="settingsbar">
        <div class="block">
            <h3>random</h3>
            <h3>weakness</h3>
            <h3>alphabet</h3>
        </div>

        <div class="block">
            <h3>time</h3>
            <h3>letters</h3>
        </div>

        <div class="block">
            <h3>10</h3>
            <h3>25</h3>
            <h3>50</h3>
            <h3>100</h3>
        </div>
    </div>

</template>

<style scoped>

.settingsbar {
    display: flex;
    flex-direction: row;
    background-color: var(--gruv-back-layer);
    height: 40px;
    align-items: center;
    border-radius: 10px;
    margin-top: 20px;

}

.block {
    display: flex;
    padding-left: 20px;
    padding-right: 20px;

}
h3 {
    color: var(--gruv-h1);
    padding-left: 8px;
    padding-right: 8px;

}
</style>