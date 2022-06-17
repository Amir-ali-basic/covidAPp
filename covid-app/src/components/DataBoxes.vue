<template>
  <h1>{{ stats.title }}</h1>
  <div class="cases">
    <div class="new-cases">
      <p>Novi potvrdjeni: {{ numberWithSpaces(stats.NewConfirmed) }}</p>
      <p>Ukupno potvrdjenih: {{ numberWithSpaces(stats.TotalConfirmed) }}</p>
    </div>
    <div class="new-deadhs">
      <p>Novi preminuli: {{ numberWithSpaces(stats.NewDeaths) }}</p>
      <p>ukupno preminulih: {{ numberWithSpaces(stats.TotalDeaths) }}</p>
    </div>
  </div>
</template>
<script>
export default {
  name: 'DataBoxes',
  props: ['stats', 'title', 'numberWithSpaces'],
};
</script>
<style lang="scss">
.cases {
  display: flex;
  width: 100%;
  justify-content: center;
  gap: 10px;
  .new-cases {
    background: #3d7051;
    padding: 15px;
    width: 310px;
  }
  .new-deadhs {
    background: #821d3f;
    padding: 15px;
    width: 310px;
  }
}
</style>
