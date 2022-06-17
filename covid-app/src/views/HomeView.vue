<template>
  <main class="home" v-if="!loading">
    <DataInfo :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" :numberWithSpaces="numberWithSpaces" />
    <CountrySelect
      @get-country="getSelectedCountryData"
      :countries="countries"
      :numberWithSpaces="numberWithSpaces"
    />
    <AllCountries :countries="countries" :numberWithSpaces="numberWithSpaces" />
  </main>
  <main v-else>Loading...</main>
</template>

<script>
import DataInfo from '@/components/DataInfo';
import DataBoxes from '@/components/DataBoxes';
import AllCountries from '@/components/AllCountries';
import CountrySelect from '@/components/CountrySelect';
import '@/assets/global.scss';
export default {
  name: 'HomeView',
  components: {
    DataInfo,
    DataBoxes,
    AllCountries,
    CountrySelect,
  },
  data() {
    return {
      loading: true,
      title: 'All world',
      dataDate: '',
      stats: {},
      countries: {},
      loading: 'Loading ...',
    };
  },
  methods: {
    async fetchCovidData() {
      try {
        const res = await fetch('https://api.covid19api.com/summary');
        return await res.json();
      } catch (error) {
        throw error;
      }
    },

    getSelectedCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    numberWithSpaces(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
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
