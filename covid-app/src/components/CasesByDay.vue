<template>
  <div>{{ Country }}</div>
</template>
<script>
export default {
  name: 'CasesByDay',
  props: ['fetchCovidDataByDay', 'dataByDay'],
};
</script>
