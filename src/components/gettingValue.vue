<template>
    <div>
      <p>{{ pattern.join(' ') }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pattern: {
        type: Array,
        default: () => [],
      },
    },
  };
  </script>
  