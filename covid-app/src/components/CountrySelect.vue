<template>
  <select 
  @change="changeCountry"
   v-model="selected">
    <option value="0">Select a Country</option>
    <option v-for="country in countries" :value="country.ID">
      {{ country.Country }}
    </option>
  </select>
  <div class="container"> 
  <div class="container" v-if="byDay">
     <div v-for="confirmed in arrConfirmed" :value="confirmed.ID">
        <h2>Positive</h2>
        <div>{{confirmed.date}} - {{confirmed.total}}</div>
      </div>
      <div v-for="deaths in arrDeaths" :value="deaths.ID">
        <h2>Negative</h2>
        <div>{{deaths.date}} - {{deaths.total}}</div>
    </div>
  </div>
  </div>
</template>
<script>
import Chart from '@/components/Chart.vue';
export default {
  name: 'CountrySelect',
  componens:{
    Chart
  },
  props: ['countries'],
  data() {
    return {
      //4475684d-c089-4f3e-8b09-d15156bc663a
      selected: 0,
      byDay:{},
      arrConfirmed:[],
      arrDeaths:[],
      arrRecovered:[],
      arrActive:[],
      chartOptions:{
        responsive: true,
        maintainAspectRatio: false,
      },
      contryByDay:{},
      name: 'global',
    };
  },
methods: {
  changeCountry() {
    const country = this.countries.find((item) => item.ID === this.selected);
    this.name = country.Country.replace(/\s+/g, '-').toLowerCase();
    this.$emit('get-country', country);
    this.updateByDay()
  },
  async fetchCovidDataByDay(){
    const res = await fetch(`https://api.covid19api.com/live/country/${this.name}/status/confirmed`);
    const data = await res.json();
    this.arrConfirmed= [];
    this.arrDeaths = [];
    this.arrRecovered = [];
    this.arrActive = [];

    data.forEach(item =>{
      const date = item.Date;
      const {
        Confirmed,
        Deaths,
        Recovered,
        Active
      } = item;

      this.arrConfirmed.push({date, total: Confirmed})
      this.arrDeaths.push({date, total: Deaths})
      this.arrRecovered.push({date, total: Recovered})
      this.arrActive.push({date, total: Active})
    })
    return data;
  },
  async updateByDay () {
    const data = await this.fetchCovidDataByDay();
    this.byDay = data;
  }
},
created() {
  this.updateByDay()
}
};
</script>
