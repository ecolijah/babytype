<template>
    <div class="titlebar">
        <div class="block">
            <img src="/src/components/icons/icons8-babys-room-64.png"/>
            <RouterLink to="/" style="text-decoration: none;" ><h1>babytype</h1></RouterLink>

        </div>

        <div>
            <nav>
                <RouterLink to="/profile" style="text-decoration: none;"><h3>profile</h3></RouterLink>
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
    width: 100%;

}
nav {
    display: flex;

}
h3 {
    padding-right: 8px;
    color: var(--gruv-titles);
}
h1 {
    font-weight: 500;
    color: var(--gruv-titles);

}


</style>