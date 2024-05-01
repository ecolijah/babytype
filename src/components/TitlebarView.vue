<template>
    <div class="titlebar">
        <div class="block">
            <img src="/src/components/icons/icons8-babys-room-64.png"/>
            <h1>babytype</h1>

        </div>

        <div>
            <h5>profile</h5>
            <h5>settings</h5>

        </div>
    </div>
</template>

<style scoped>
img {
    max-height: 48px;
}
.block {
    display: flex;
    align-items: center;
}
.titlebar {
    display: flex;
    justify-content: space-between;
    color: var(--gruv-titles);
    width: 100%;
    /* background-color: yellowgreen; */

}
</style>