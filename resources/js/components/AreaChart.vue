<template>
  <div class="container pt-5">
    <h3>Area chart</h3>
    <!-- Area Chart Element -->
    <div id="chart">
      <apexchart
        type="area"
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
  name: "AreaChart",
  components: {
    apexchart: VueApexCharts,
  },
  props:{
    prices:[],
    time:[],
  },
  data() {
    return {
      series: this.prices,
      chartOptions: {
        chart: {
          type: "area",
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: "smooth",
        },
        title: {
          text: "Market Prices",
          align: "left",
        },
        xaxis: {
          type: "datetime",
          categories: this.time,
          labels: {
            formatter: (val) => {
              return dayjs(val).format("MMM DD HH:MM");
            },
          },
        },
      },
    };
  },
  
};
</script>

<style lang="scss" scoped>
</style>