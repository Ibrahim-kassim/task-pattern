<template>
    <div>
      <CircleGrid @update-pattern="updatePattern" />
      <div>
        your numbers of code pattern is
        <gettingValue :pattern="pattern" />
      </div>
     
    </div>
  </template>
  
  <script>
  import CircleGrid from './CircleGrid.vue';
  import gettingValue from './gettingValue.vue';
  
  export default {
    components: {
        CircleGrid,
        gettingValue,
    },
    data() {
      return {
        pattern: [],
      };
    },
    methods: {
      updatePattern(newPattern) {
        this.pattern = newPattern;
      },
    },
  };
  </script>
  