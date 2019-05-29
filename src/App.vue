<template>
  <div id="app">
    <h1><u>QR Scanner</u></h1>
    <Scanner />
  </div>
</template>

<script>
import Scanner from './components/Scanner.vue'

export default {
  name: 'app',
  components: {
    Scanner
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}
</style>
