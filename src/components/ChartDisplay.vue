<template>
    <div v-if="chartData.length > 0" ref="chartdiv" class="hello"></div>
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
    createChart(){
      let chart = am4core.create(this.$refs.chartdiv, am4charts.XYChart);
      chart.paddingRight = 20;
      
      chart.data = this.chartData;
      console.log(this.chartData);
      let xAxis = chart.xAxes.push(new am4charts.DateAxis());
      let yAxis = chart.yAxes.push(new am4charts.CategoryAxis());

      xAxis.dataFields.dateX = "date";
      yAxis.dataFields.category = "hour";
      yAxis.title.text="Hour"

      // xAxis.renderer.grid.template.disabled = true;
      // xAxis.renderer.minGridDistance = 40;

      // yAxis.renderer.grid.template.disabled = true;
      // yAxis.renderer.inversed = true;
      // yAxis.renderer.minGridDistance = 30;

      // Create Series: associate each key-value pair with a visual
      let series = chart.series.push(new am4charts.ColumnSeries()); 
      series.dataFields.dateX = "date";
      series.dataFields.categoryY = "hour";
      series.dataFields.value = "impressions";
      series.name = "Impressions";
      series.tooltipText = "{name}: [bold]{valueY}[/]";
      // series.stacked = true;
      // series.sequencedInterpolation = true;
      // series.columns.template.width = am4core.percent(100);
      // series.columns.template.height = am4core.percent(100);

      // series.heatRules.push({target:series.columns.template, property:"fill", min:am4core.color("#ffffff"), max:am4core.color("#692155")});

      // var columnTemplate = series.columns.template;
      // columnTemplate.strokeWidth = 2;
      // columnTemplate.strokeOpacity = 1;
      // columnTemplate.stroke = am4core.color("#ffffff");
      // columnTemplate.tooltipText = "{date.getDate()}, {hour}: {value.workingValue.formatNumber('#.')}";

    //   // heat legend
    //   var heatLegend = chart.bottomAxesContainer.createChild(am4charts.HeatLegend);
    //   heatLegend.width = am4core.percent(100);
    //   heatLegend.series = series;
    //   heatLegend.valueAxis.renderer.labels.template.fontSize = 9;
    //   heatLegend.valueAxis.renderer.minGridDistance = 30;

    //   // heat legend behavior
    //   series.columns.template.events.on("over", (event) => {
    //     handleHover(event.target);
    //   })

    //   series.columns.template.events.on("hit", (event) => {
    //     handleHover(event.target);
    //   })

    //   function handleHover(column) {
    //     if (!isNaN(column.dataItem.value)) {
    //       heatLegend.valueAxis.showTooltipAt(column.dataItem.value)
    //     }
    //     else {
    //       heatLegend.valueAxis.hideTooltip();
    //     }
    //   }
    //   series.columns.template.events.on("out", (event) => {
    //     heatLegend.valueAxis.hideTooltip();
    //   })
    //   this.chart = chart;
    }
  },
  
  async mounted(){
    console.log(this.chartData.length >0);
    await this.getData()
    if(this.chartData.length >0){
      this.createChart();
    } else {

    }
  }
}
</script>