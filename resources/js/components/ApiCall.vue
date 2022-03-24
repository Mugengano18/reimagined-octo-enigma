<template>
  <div>
      <history-chart v-if="allInfo.length > 0" :name="name" :allInfo="allInfo"/>
    <area-chart
      v-if="prices.length > 0 && time.length > 0"
      :prices="prices"
      :time="time"
    />
    <line-chart
      v-if="prices.length > 0 && time.length > 0"
      :prices="prices"
      :time="time"
    />
    <column-chart
      v-if="prices.length > 0 && time.length > 0"
      :prices="prices"
      :time="time"
    />
  </div>
</template>

<script>
import AreaChart from "./AreaChart.vue";
import ColumnChart from "./ColumnChart.vue";
import LineChart from "./LineChart.vue";
import HistoryChart from "./HistoryChart.vue"
export default {
  name: "ApiCall",
  components: {
    AreaChart,
    ColumnChart,
    LineChart,
    HistoryChart
  },
  data() {
    return {
      prices: [],
      time: [],
      name:"Prices",
      allInfo:[]
    };
  },
  methods: {
    async getData() {
      // variable holding the api and all it's params and headers.
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
      // Axios usage to fetch the data from the api
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
          this.allInfo = [
            {
              name: "prices",
              data: info[0],
            },
          ];
          //   getting each element in one array and sending them to the data function elements
          this.prices = [
            {
              name: "Open Price",
              data: openprice,
            },
            {
              name: "Highest Price",
              data: highprice,
            },
            {
              name: "Lowest Price",
              data: lowprice,
            },
            {
              name: "Closed Price",
              data: closeprice,
            },
          ];
          this.time = category;
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