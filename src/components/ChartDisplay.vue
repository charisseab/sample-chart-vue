<template>
    <div v-if="chartData.length > 0">
      <div ref="chartdiv" class="hello"></div>
    </div>
    
    <p v-else>{{message}}</p>
</template>

<script>
import * as Vue from 'vue' 
import axios from 'axios'
import VueAxios from 'vue-axios'

import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

export default {
  name: "ChartDisplay",
  props: {
    metric: String
  },
  data() {
    return{
      baseUrl: import.meta.env.VITE_API_URL,
      message: "No data available",
      chartData: []
    }
  },
  
  methods: {
    async getData(){
      console.log("this.baseUrl");
      console.log(this.baseUrl+"/stats/hourly");
      await axios.get(this.baseUrl+"/stats/hourly").then((response) => {
        console.log(response.data)
        this.chartData = response.data
      })
    },
    selectMetrics(){

    },
    createChart(){
      let chart = am4core.create(this.$refs.chartdiv, am4charts.XYChart);
      chart.paddingRight = 20;
      
      chart.data = this.chartData;
      console.log(this.chartData);
      let xAxis = chart.xAxes.push(new am4charts.DateAxis());
      let yAxis = chart.yAxes.push(new am4charts.ValueAxis());

      xAxis.dataFields.dateX = "date";
      yAxis.title.text="Figure"

      let series = chart.series.push(new am4charts.ColumnSeries());
      series.dataFields.dateX = "date";
      series.dataFields.valueY = this.metric;
      series.name = this.metric.toUpperCase();
      series.tooltipText = "{name}";

      // Legend and cursor
      chart.cursor = new am4charts.XYCursor();
      chart.legend = new am4charts.Legend();
    },
    async loadChart(){
      await this.getData()
      if(this.chartData.length >0){
        this.createChart();
      }
    }
  },

  async mounted(){
    this.loadChart()
  },

  watch: {
    async metric(newProp, oldProp){
      this.loadChart()
    }
  }
}
</script>

<style>
.hello {
  width: 100%;
  height: 500px;
}
.metrics-wrap{
  margin-bottom: 2rem;
}
</style>