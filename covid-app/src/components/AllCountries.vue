<template>
  <div class="btn-container">
    <button v-on:click="sortByLowest()">Sortiraj najmanje ukupnih</button>
    <button v-on:click="sortByLargest()">Sortiraj najvise ukupnih</button>
    <button v-on:click="sortByAtoZ()">Sortiraj A-Z</button>
    <button v-on:click="sortByZtoA()">Sortiraj Z-A</button>
  </div>

  <table>
    <thead>
      <tr>
        <th colspan="1">Naziv zemlje</th>
        <th colspan="2">Slucajevi</th>
        <th colspan="2">Preminuli</th>
      </tr>
      <tr>
        <td scope="col">Naziv zemlje</td>
        <td scope="col">Novi potvrdjeni</td>
        <td scope="col">Ukupno</td>
        <td scope="col">Novi preminuli</td>
        <td scope="col">Ukupno preminulih</td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="country in countries" v-bind:key="country.id">
        <td>
          {{ country.Country }}
        </td>
        <td>{{ numberWithSpaces(country.NewConfirmed) }}</td>
        <td>{{ numberWithSpaces(country.TotalConfirmed) }}</td>
        <td>{{ numberWithSpaces(country.NewDeaths) }}</td>
        <td>{{ numberWithSpaces(country.TotalDeaths) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'AllCountries',
  props: ['countries', 'numberWithSpaces'],
  data() {
    return {
      name: '',
    };
  },
  methods: {
    sortByLowest() {
      this.countries.sort((a, b) =>
        a.TotalConfirmed > b.TotalConfirmed ? 1 : -1
      );
    },
    sortByLargest() {
      this.countries.sort((a, b) =>
        a.TotalConfirmed < b.TotalConfirmed ? 1 : -1
      );
    },
    sortByAtoZ() {
      this.countries.sort((a, b) => a.Country.localeCompare(b.Country));
    },
    sortByZtoA() {
      this.countries.sort((a, b) => b.Country.localeCompare(a.Country));
    },
  },
};
</script>
<style land="scss">
table {
  width: 100%;
}
table,
tr,
td {
  border-bottom: 1px solid #fff;
}
.btn-container {
  display: flex;
  gap: 10px;
  margin-top: 15px;
  margin-bottom: 5px;
}

button {
  height: 35px;
  background: transparent;
  color: white;
  border: 2px solid white;
  border-radius: 50px;
}
</style>
