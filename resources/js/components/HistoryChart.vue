<template>
  <div class="container">
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
import moment from "moment";
import dayjs from "dayjs";
import VueApexCharts from "vue-apexcharts";
export default {
  name: "HistoryChart",
  components: {
    apexchart: VueApexCharts,
  },
  data() {
    return {
      series: [
        {
          name: "prices",
          data: [],
        },
      ],
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
            formatter: function (val) {
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
          category.map((cat, index) => {
            let h = highprice[index];
            let l = lowprice[index];
            let o = openprice[index];
            let c = closeprice[index];
            // pushing single item
            item.push({
              x: cat,
              y: [o, h, l, c],
            });
          });
          info.push(item);
          console.log(info[0]);
          this.series = [
            {
              data: info[0],
            },
          ];
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