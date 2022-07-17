

<template>
  <div class="greetings">
    <h1 class="green">Chart</h1>
    <div class="metrics-wrap">
      <label>
        Select Metric:
      </label>
      <select id="metricSelectOptions" >
        <option value="impressions">Impression</option>
        <option value="clicks">Clicks</option>
        <option value="revenue">Revenue</option>
      </select>
      <button v-on:click="selectMetric">View Metric</button>
    </div>
    <ChartDisplay :metric="selectedMetric" />
  </div>
</template>

<script>

import ChartDisplay from './ChartDisplay.vue'

export default {
  name: 'ChartsPage',
  components: {
    ChartDisplay
  },
  data(){
    return{
      selectedMetric: "impressions"
    }
  },  
  methods: {
    selectMetric(){
      this.selectedMetric = document.getElementById("metricSelectOptions").value
    }
  }, 
}
</script>

<style scoped>
#chartdiv {
  width: 100%;
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
