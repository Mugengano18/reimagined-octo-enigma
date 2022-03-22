<template>
  <div class="container">
    <h1>Area chart</h1>
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
         const newData =forex_data[0].slice(0,100);
          console.log(newData);
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
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
         const newData =forex_data[0].slice(0,100);
          console.log(newData);
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
  },
  mounted() {
    this.getDate();
  },
};
</script>

<style lang="scss" scoped>
</style>