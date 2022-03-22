<template>
  <div class="container">
    <h1>Area chart</h1>
    <!-- date element -->
    <div class="row">
      <div class="col-12">
        <input
          type="date"
          v-model="date"
          @change="getInfo"
          class="float-right"
        />
      </div>
    </div>
    <!-- chart element -->
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
import VueApexCharts from "vue-apexcharts";
export default {
  name: "AreaChart",
  components: {
    apexchart: VueApexCharts,
  },
  // chart variable declations
  data: function() {
      return {
        chartOptions: {
          chart: {
            id: 'vuechart-example'
          },
          xaxis: {
            categories: []
          }
        },
        series: [

          {
          name: 'open price',
          data: []
        },
          {
          name: 'Highest Price',
          data: []
        },
        {
          name: 'Lowest price',
          data: []
        },
        {
          name: 'close price',
          data: []
        },
        ],
        date:"",
      }
    },
  methods: {
    // getting the info to the areachart
    async getInfo() {
      axios
        .get(
          `https://api.polygon.io/v2/aggs/grouped/locale/global/market/fx/${this.date}?adjusted=true&apiKey=dbtPb51SUYmGj6mbSvN38tDjDCtopc5G`
        )
        .then((response) => {
          let high = [];
          let low = [];
          let category = [];
          let open = [];
          let close = [];
          this.forex_data = response.data.results;
          for (let i = 0; i < response.data.results.length; i++) {
            high.push(response.data.results[i].h),
            category.push(response.data.results[i].T),
            low.push(response.data.results[i].l);

          }
          this.series = [
            {
              data:open
            },
            {
          data: high
          },
          {
            data:low
          },
          {
            data:close
          }
          ];
        this.chartOptions = {
          xaxis:{
            categories:category
          }
        };
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
    // this is setting the today's date when mounted
    getDate() {
      let yourDate = new Date();
      let today = yourDate.toISOString().split("T")[0];
      this.date = today;
      axios
        .get(
          `https://api.polygon.io/v2/aggs/grouped/locale/global/market/fx/${today}?adjusted=true&apiKey=dbtPb51SUYmGj6mbSvN38tDjDCtopc5G`
        )
        .then((response) => {
          let high = [];
          let low = [];
          let category = [];
          let open = [];
          let close = [];
          this.forex_data = response.data.results;
          for (let i = 0; i < response.data.results.length; i++) {
            high.push(response.data.results[i].h),
            category.push(response.data.results[i].T),
            low.push(response.data.results[i].l);

          }
          // passing the api's data to the chart elements
          this.series = [
            {
              data:open
            },
            {
          data: high
          },
          {
            data:low
          },
          {
            data:close
          }
          ];
        this.chartOptions = {
          xaxis:{
            categories:category
          }
        };
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
  },
  mounted() {
    // the method to get the date
    this.getDate();
  },
};
</script>

<style lang="scss" scoped>
</style>