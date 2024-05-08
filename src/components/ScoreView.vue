<template>
    <div class="statscreen">
      <div class="block">
          <h2>accuracy</h2>
          <h1>{{ accuracy}}%</h1>
      </div>

      <div class="block">
        <h2>missed</h2>
        <!-- <h1> {{ missedmap }}</h1> -->
        <h1 v-for="item in missedmap" :key="item">{{ item[0]}}</h1>
      </div>
      
      <p>press <span>Tab</span> to begin next test</p>

    </div>
</template>
<script setup>
const props = defineProps(['accuracy', 'missedmap'])
console.log(props)
</script>

<style scoped>
.statscreen {
  display: flex;
  align-items: center;
  /* flex-direction: column; */
  flex-direction: row;
  justify-content: center;
  color: var(--gruv-h1);
  height: 70vh;
  background-color: aliceblue;
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
  font-size: 4em;
  font-weight: 400;
  /* background-color: brown; */
}
h2 {
  font-weight: 400;
  font-size: 2em;
  margin: 0;

}
.block {
  /* background-color: aliceblue; */
  padding: 20px;
}

</style>