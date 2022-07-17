

<template>
  <div class="greetings">
    <h1 class="green">Chart</h1>
    <ChartDisplay/>
  </div>
</template>

<script>

import ChartDisplay from './ChartDisplay.vue'

export default {
  name: 'ChartsPage',
  components: {
    ChartDisplay
  },
  
}
</script>

<style scoped>
#chartdiv {
  width: 100%;
  height: 400px;
}
.hello {
  width: 100%;
  height: 500px;
}
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
