<!-- App.vue -->

<template>
  <div id="app">
   
    <CircleGrid/>
   
  </div>
</template>

<script>
import CircleGrid from './components/CircleGrid.vue';

export default {
  name: 'App',
  components: {
    CircleGrid
  }
}
</script>

<style>
#app {
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
