<!-- App.vue -->

<template>
  <div id="app">
    <gettingValue/>
    <updateValue/>
   
  </div>
</template>

<script>
import gettingValue from "./components/gettingValue.vue"
import updateValue from "./components/updateValue.vue"

export default {
  name: 'App',
  components: {

    gettingValue,
    updateValue
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
