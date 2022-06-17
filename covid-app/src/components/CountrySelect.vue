<template>
<div class="selectContainer">
  <select 
  @change="changeCountry"
   v-model="selected">
    <option value="0">Odaberite zemlju</option>
    <option v-for="country in countries" :value="country.ID">
      {{ country.Country }}
    </option>
  </select>
  </div>
  <div class="container grid" v-if="selected !== 0">
  <div>
    <h2>Novi slucajevi</h2>
      <div v-for="confirmed in arrConfirmed" :value="confirmed.ID">
        <div>{{(confirmed.date)}} - {{numberWithSpaces(confirmed.total)}}</div>
    </div>
  </div>

    <div>
    <h2>Novi oporavljeni</h2>
    <div v-for="recovered in arrRecovered" :value="recovered.ID">
      <div>{{(recovered.date)}} - {{numberWithSpaces(recovered.total)}}</div>
    </div>
  </div>

  <div>
    <h2>Novi preminuli</h2>
    <div v-for="deaths in arrDeaths" :value="deaths.ID">
      <div>{{(deaths.date)}} - {{numberWithSpaces(deaths.total)}}</div>
    </div>
  </div>

  </div>
</template>
<script>
import moment from 'moment';
export default {
  name: 'CountrySelect',

  props: ['countries', 'numberWithSpaces'],
  data() {
    return {
      selected: 0,
      byDay:{},
      arrConfirmed:[],
      arrDeaths:[],
      arrRecovered:[],
      arrActive:[],
      contryByDay:{},
      name: 'global',
    };
  },
methods: {
  changeCountry() {
    const country = this.countries.find((item) => item.ID === this.selected);
    this.name = country.Country.replace(/\s+/g, '-').toLowerCase();
    console.log(this.name);
    this.$emit('get-country', country);
    this.updateByDay()
  },
  async fetchCovidDataByDay(){
    try {
    const res = await fetch(`https://api.covid19api.com/live/country/${this.name}/status/confirmed`);
    const data = await res.json();
    this.arrConfirmed = [];
    this.arrDeaths = [];
    this.arrRecovered = [];
    this.arrActive = [];
    data.forEach((item, i) =>{
      const date = moment(item.Date).format('MMMM Do YYYY');
      const {
        Confirmed,
        Deaths,
        Recovered,
        Active
      } = item;
      //racunamo kioliko potvrdjenih slucajeva ima u jednom danu tako sto oduzimamo od prethodnog, 
      //dakle ukupan broj slucajeva - ukupan broj slucajeva prosli dan
      this.arrConfirmed.reverse().push({date, total: Confirmed - (data[i - 1]?.Confirmed || 0)  });
      this.arrDeaths.reverse().push({date, total: Deaths - (data[i - 1]?.Deaths || 0)})
      //nisam bio siguran da li sam trebao da prikazem ukupan broj potvrdjenih slucajeva ko sto nudi api ili 
      //koliko je bilo samo tog dana
      this.arrRecovered.reverse().push({date, total: Recovered})
      this.arrActive.reverse().push({date, total: Active})
      console.log(this.arrConfirmed)
    })
    return data;
    }
    catch(error){
      console.log(error)
    }

  },
  async updateByDay () {
    const data = await this.fetchCovidDataByDay();
    this.contryByDay = data;
  },
},
created() {
  this.updateByDay()
}
};
</script>
<style>
.selectContainer{
  width: 100%;
  display: flex;
  justify-content: center;
}
select{
  margin-top:10px;
  background:#041C32;
  border:2px solid #ffff;
  height: 45px;
  color: #ffff;
  border-radius: 50px;
  padding:10px;
  outline:none;
}
</style>