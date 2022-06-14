<template>
  <h1>{{ stats.title }}</h1>
  <div class="grid">
    <div class="new-cases">
      <p>New confirmed: {{ numberWithSpaces(stats.NewConfirmed) }}</p>
      <p>Total confirmed: {{ numberWithSpaces(stats.TotalConfirmed) }}</p>
    </div>
    <div class="new-cases">
      <p>NewDeaths: {{ numberWithSpaces(stats.NewDeaths) }}</p>
      <p>TotalDeaths: {{ numberWithSpaces(stats.TotalDeaths) }}</p>
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
.grid {
  display: grid;
  grid-template-columns: repeat(2, 2fr);
  grid-template-rows: auto;
  justify-content: center;
  width: 100%;
  .new-cases {
    background: pink;
  }
  .new-deadhs {
    background: red;
  }
}
</style>
