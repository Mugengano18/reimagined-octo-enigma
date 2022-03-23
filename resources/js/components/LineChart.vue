<template>
  <div class="container">
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
  data() {
    return {
      series: [
        {
          name: "Open Price",
          data: [],
        },
        {
          name: "Highest Price",
          data: [],
        },
        {
          name: "Lowest Price",
          data: [],
        },
        {
          name: "Closed Price",
          data: [],
        },
      ],
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
          categories: [],
          labels: {
            formatter: function (val) {
              return dayjs(val).format("MMM DD HH:MM");
            },
          },
        },
      },
    };
  },
  methods: {
    async getData() {
      let options = {
        method: "GET",
        url: "https://yh-finance.p.rapidapi.com/stock/v3/get-historical-data",
        params: { symbol: "AMRN", region: "US" },
        headers: {
          "X-RapidAPI-Host": "yh-finance.p.rapidapi.com",
          "X-RapidAPI-Key":
            "3a85e2a9femshc75f599f0f8d052p1cdebajsn8b711f561255",
        },
      };

      axios
        .request(options)
        .then((response) => {
          // all array variable declarion
          let highprice = [];
          let lowprice = [];
          let category = [];
          let closeprice = [];
          let openprice = [];
          let info = [];
          let item = [];
          for (let i = 0; i < response.data.prices.length; i++) {
            category.push(response.data.prices[i].date);
            openprice.push(response.data.prices[i].open);
            highprice.push(response.data.prices[i].high);
            lowprice.push(response.data.prices[i].low);
            closeprice.push(response.data.prices[i].close);
          }
          //   getting each element in one array
          this.series = [
            {
              data: openprice,
            },
            {
              data: highprice,
            },
            {
              data: lowprice,
            },
            {
              data: closeprice,
            },
          ];
          this.chartOptions = {
            xaxis: {
              categories: category,
            },
          };
        })
        .catch(function (error) {
          console.error(error);
        });
    },
  },
  mounted() {
    this.getData();
  },
};
</script>

<style lang="sass" scoped>
</style>