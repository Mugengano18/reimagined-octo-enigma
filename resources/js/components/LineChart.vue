<template>
  <div class="container pt-5">
      <h3>Line chart</h3>
    <div id="chart">
      <apexchart
        type="line"
        height="500"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import dayjs from "dayjs";
import VueApexCharts from "vue-apexcharts";
export default {
  name: "LineChart",
  components: {
    apexchart: VueApexCharts,
  },
  props:{
    prices:[],
    time:[]
  },
  data() {
    return {
      series: this.prices,
      chartOptions: {
        chart: {
          type: "line",
          zoom: {
            enabled: true,
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: "straight",
        },
        title: {
          text: "Market Prices",
          align: "left",
        },
        xaxis: {
          type: "datetime",
          categories: this.time,
          labels: {
            formatter:(val) => {
              return dayjs(val).format("MMM DD HH:MM");
            },
          },
        },
      },
    };
  },
};
</script>

<style lang="sass" scoped>
</style>