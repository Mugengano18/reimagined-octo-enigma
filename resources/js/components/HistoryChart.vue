<template>
  <div class="container pt-5">
    <h3>CandleStick chart</h3>
    <!-- CandleStick Chart -->
    <div id="chart">
      <apexchart
        type="candlestick"
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
  name: "HistoryChart",
  components: {
    apexchart: VueApexCharts,
  },
  props:{
    allInfo:[]
  },
  data() {
    return {
      series: this.allInfo,
      chartOptions: {
        chart: {
          type: "candlestick",
        },
        title: {
          text: "Market prices",
          align: "left",
        },
        tooltip: {
          enabled: true,
        },
        xaxis: {
          type: "datetime",
          labels: {
            formatter:(val) => {
              return dayjs(val).format("MMM DD HH:MM");
            },
          },
        },
        yaxis: {
          tooltip: {
            enabled: true,
          },
        },
      },
    };
  },
};
</script>

<style lang="scss" scoped>
</style>