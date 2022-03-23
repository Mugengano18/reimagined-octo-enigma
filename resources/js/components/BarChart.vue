<template>
  <div class="container">
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
import moment from "moment";
import dayjs from "dayjs";
import VueApexCharts from "vue-apexcharts";
export default {
  name: "Barchart",
  components: {
    apexchart: VueApexCharts,
  },
  data() {
    return {
      series: [
        {
          name: "Open Prices",
          data: [],
        },
        {
          name: "Highest Prices",
          data: [],
        },
        {
          name: "Lowest Prices",
          data: [],
        },
        {
          name: "Closed prices",
          data: [],
        },
      ],
      chartOptions: {
        chart: {
          type: "bar",
        },
        plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: "55%",
            endingShape: "rounded",
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          show: true,
          width: 2,
          colors: ["transparent"],
        },
        xaxis: {
          type: "datetime",
          categories: [],
          labels: {
            formatter: function (val) {
              return dayjs(val).format("MMM DD HH:mm");
            },
          },
        },
        yaxis: {
          title: {
            text: "$ (prices)",
          },
        },
        fill: {
          opacity: 1,
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

<style lang="scss" scoped>
</style>