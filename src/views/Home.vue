<template>
  <main v-if="!loading">
    <data-title :text="title" :dataDate="dataDate"></data-title>
    <data-boxes :stats="stats"></data-boxes>
    <country-select
      :countries="countries"
      @GetCountry="getCountryData"
    ></country-select>
    <button
      @click="clearCountryData"
      v-if="stats.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600"
    >
      Clear Country
    </button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="test-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>

<script>
import DataBoxes from "../components/DataBoxes.vue";
import DataTitle from "../components/DataTitle.vue";
import CountrySelect from "../components/CountrySelect.vue";
// @ is an alias to /src

export default {
  name: "Home",
  components: { DataTitle, DataBoxes, CountrySelect },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/hourglass.webp"),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      return data;
    },
    getCountryData(country) {
      this.dataDate = country.Date;
      this.title = country.Country;
      this.stats = country;
      this.loading = false;
    },
    async clearCountryData() {
      this.loading = true;
      const data = await this.fetchCovidData();

      this.title = "Global";
      this.dataDate = data.Date;
      this.stats = data.Global;
      this.countries = data.Countries;
      this.loading = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>
