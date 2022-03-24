<template>
  <div class="container pt-5">
    <h3>ColumnChart</h3>
    <div id="chart">
      <apexchart
        type="bar"
        height="500"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
import VueApexCharts from "vue-apexcharts";
export default {
  name: "Columnchart",
  components: {
    apexchart: VueApexCharts,
  },
  props:{
    prices:[],
    time:[]
  },
  data() {
    return {
      series:this.prices,
      chartOptions: {
        chart: {
          type: "bar",
        },
        plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: "100%",
            endingShape: "rounded",
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          show: true,
          width: 3,
          colors: ["transparent"],
        },
        xaxis: {
          type: "datetime",
          categories: this.time,
          labels: {
            formatter:(val) => {
              return dayjs(val).format("MMM DD HH:mm");
            },
          },
        },
        tooltip: {
          enabled:true,
        },
        yaxis: {
          tickAmount:6,
        },
        fill: {
          opacity: 1,
        },
      },
    };
  },
};
</script>

<style lang="scss" scoped>
</style>