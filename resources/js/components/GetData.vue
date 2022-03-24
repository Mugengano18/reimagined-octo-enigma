<template>
  <div>
      <pie-chart v-if="x_elements.length !==0" :x_elements="x_elements" :y_elements="y_elements"/>
  </div>
</template>

<script>
import PieChart from "./PieChart.vue";
export default {
  name: "GetData",
  components: {
    PieChart,
  },
  data() {
    return {
        x_elements:[],
        y_elements:["Confirmed Cases","Deaths Cases","recovered Patients"]
    };
  },
  methods: {
    async getInfo() {
      var options = {
        method: "GET",
        url: "https://coronavirus-smartable.p.rapidapi.com/stats/v1/US/",
        headers: {
          "X-RapidAPI-Host": "coronavirus-smartable.p.rapidapi.com",
          "X-RapidAPI-Key":
            "3a85e2a9femshc75f599f0f8d052p1cdebajsn8b711f561255",
        },
      };

      axios
        .request(options)
        .then((response) => {
            let confirmed = response.data.stats.totalConfirmedCases;
            let deaths = response.data.stats.totalDeaths;
            let recovered = response.data.stats.totalRecoveredCases;
            const allStats = new Array(confirmed,deaths,recovered)
            this.x_elements = allStats
            console.log(allStats)
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
  mounted(){
      this.getInfo();
  }
};
</script>

<style lang="sass" scoped>
</style>