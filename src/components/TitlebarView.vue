<template>
    <div class="titlebar">
        <div class="block">
            <img src="/src/components/icons/icons8-babys-room-64.png"/>
            <h1>babytype</h1>

        </div>

        <div>
            <nav>
                <RouterLink to="/">home</RouterLink>
                <RouterLink to="/profile">profile</RouterLink>
            </nav>
        </div>
    </div>
</template>

<style scoped>
img {
    max-height: 40px;
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

h1 {
    font-weight: 500;
}
</style>