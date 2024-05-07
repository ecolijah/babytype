<template>
    <div class="statscreen">
      <div class="block">
          <h2>accuracy</h2>
          <h1>{{ accuracy}}%</h1>
      </div>
      
      <p>press <span>Tab</span> to begin next test</p>

    </div>
</template>
<script setup>
const props = defineProps(['accuracy'])
console.log(props.accuracy)
</script>

<style scoped>
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
  color:  var(--gruv-background);
  font-weight: 700;
  background-color: var(--gruv-h1);
  padding-left: 3px;
  padding-right: 3px;
  border-radius: 3px;
}

h1 {
  color: var(--gruv-accent);
  font-size: 4.5em;
  font-weight: 500;
  /* background-color: brown; */
}
h2 {
  font-weight: 400;
  font-size: 2em;
  margin: 0;

}
.block {
  /* background-color: aliceblue; */
}

</style>