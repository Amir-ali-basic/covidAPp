<template>
  <div class="title">
    <h2>{{ text }}</h2>
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
