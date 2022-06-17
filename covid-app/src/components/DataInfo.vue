<template>
  <div class="title">
    <h1>{{ text }}</h1>
    <p>{{ timestamp }}</p>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'Title',
  props: ['text', 'dataDate', 'numberWithSpaces'],
  computed: {
    timestamp: function () {
      return moment(this.dataDate).format('MMMM Do YYYY');
    },
  },
};
</script>
<style lang="scss">
.title {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
  flex-direction: column;
  h1 {
    font-size: 3rem;
    margin-bottom: 0;
  }
  p {
    font-style: italic;
  }
}
</style>
