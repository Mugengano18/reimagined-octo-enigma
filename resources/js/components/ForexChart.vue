<template>
  <div class="container">
    <h1 class="pt-3">Grouped Daily (Bars)</h1>
    <!-- Date -->
    <div class="row">
      <div class="col-12">
        <input
          id="maximum"
          type="date"
          v-model="date"
          @change="getInfo"
          max="2022-03-22"
          class="float-right"
        />
      </div>
    </div>
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
import VueApexCharts from "vue-apexcharts";
export default {
  name: "ForexChart",
  components: {
    apexchart: VueApexCharts,
  },
  data: function () {
    return {
      // chart's data declaration
      series: [
        {
          name: "prices",
          data: [],
        },
      ],
      chartOptions: {
        chart: {
          height: "auto",
          type: "candlestick",
        },
        title: {
          text: "Forex-Data",
          align: "left",
        },
        annotations: {
          xaxis: [
            {
              borderColor: "#00E396",
              label: {
                borderColor: "#00E396",
                style: {
                  fontSize: "12px",
                  color: "#fff",
                  background: "#00E396",
                },
                orientation: "horizontal",
                offsetY: 7,
              },
            },
          ],
        },
        tooltip: {
          enabled: true,
        },
        xaxis: {
          type: "category",
          
        },
        yaxis: {
          tickAmount:15,
          logarithmic:false,
          tooltip: {
            enabled: true,
          },
        },
      },
      date: "",
    };
  },
  methods: {
    // get Api data and intergrate it
    async getInfo() {
      axios
        .get(
          `https://api.polygon.io/v2/aggs/grouped/locale/global/market/fx/${this.date}?adjusted=true&apiKey=dbtPb51SUYmGj6mbSvN38tDjDCtopc5G`
        )
        .then((response) => {
          // all array variable declarion
          let high = [];
          let low = [];
          let category = [];
          let close = [];
          let open = [];
          let data = [];
          let item = [];
          for (let i = 0; i < response.data.results.length; i++) {
            category.push(response.data.results[i].T.slice(2));
            open.push(response.data.results[i].o);
            high.push(response.data.results[i].h);
            low.push(response.data.results[i].l);
            close.push(response.data.results[i].c);
          }

          // getting each element in one array
          category.map((cat, index) => {
            let h = high[index];
            let l = low[index];
            let o = open[index];
            let c = close[index];
            // pushing single item
            item.push({
              x: cat,
              y: [o, h, l, c],
            });
          });
          data.push(item);
          const newData = data[0].slice(0, 100);

          // this.series = [
          //   {
          //     data: data[0],
          //   },
          // ];
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
    // this is to bring today's data once the page is mounted
    getDate() {
      let yourDate = new Date();
      let month = yourDate.getMonth() + 1;
      let year = yourDate.getUTCFullYear();
      let tDate = yourDate.getDate();
      if (month < 10) {
        month = "0" + month;
      }
      if (tDate < 10) {
        tDate = "0" + tDate;
      }
      let maxDate = year + "-" + month + "-" + tDate;
      // this is to disable future dates..
      document.getElementById("maximum").setAttribute("max", maxDate);
      this.date = maxDate;
      console.log(maxDate);
      axios
        .get(
          `https://api.polygon.io/v2/aggs/grouped/locale/global/market/fx/${maxDate}?adjusted=true&apiKey=dbtPb51SUYmGj6mbSvN38tDjDCtopc5G`
        )
        .then((response) => {
          // all array variable declarion
          let high = [];
          let low = [];
          let category = [];
          let close = [];
          let open = [];
          let data = [];
          let item = [];
          for (let i = 0; i < response.data.results.length; i++) {
            category.push(response.data.results[i].T.slice(2));
            open.push(response.data.results[i].o);
            high.push(response.data.results[i].h);
            low.push(response.data.results[i].l);
            close.push(response.data.results[i].c);
          }
          // getting each element in one array
          category.map((cat, index) => {
            let h = high[index];
            let l = low[index];
            let o = open[index];
            let c = close[index];
            // pushing single item
            item.push({
              x: cat,
              y: [o, h, l, c],
            });
          });
          data.push(item);
          const newData = data[0].slice(0, 100);
          this.series = [
            {
              data: data[0],
            },
          ];
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
  },
  mounted() {
    // calling today's date function
    this.getDate();
  },
};
</script>

<style lang="scss" scoped>
</style>