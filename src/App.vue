<template>
  <div id="app">
    <optimizely-info/>
  </div>
</template>

<script>
import OptimizelyInfo from './components/OptimizelyInfo'

export default {
  name: 'app',
  components: {
    OptimizelyInfo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 60px;
}
</style>
