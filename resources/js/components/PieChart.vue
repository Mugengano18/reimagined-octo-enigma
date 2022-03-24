<template>
  <div class="container">
      <h3>Pie Chart</h3>
    <div id="chart">
      <apexchart
        type="pie"
        width="380"
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
  name: "PieChart",
  components: {
    apexchart: VueApexCharts,
  },
  props:{
      x_elements:[],
      y_elements:[],
  },
  data() {
    return {
      series: this.x_elements,
      chartOptions: {
        chart: {
          width: 500,
          type: "pie",
        },
        labels: this.y_elements,
        responsive: [
          {
            breakpoint: 480,
            options: {
              chart: {
                width: 500,
              },
              legend: {
                position: "bottom",
              },
            },
          },
        ],
      },
    };
  },
  
};
</script>

<style lang="scss" scoped>
</style>