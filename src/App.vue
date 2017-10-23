<template>
  <div id="app">
    <section class="hero is-primary is-bold">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Returns
          </h1>
          <h2 class="subtitle">
            A / B / C
          </h2>
        </div>
      </div>
    </section>
    <optimizely-info/>
    <section class="hero is-primary is-bold">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Best Sellers
          </h1>
          <h2 class="subtitle">
            A / B
          </h2>
        </div>
      </div>
    </section>
    <best-sellers-info/>
  </div>
</template>

<script>
import OptimizelyInfo from './components/OptimizelyInfo'
import BestSellersInfo from './components/BestSellersInfo'

export default {
  name: 'app',
  components: {
    OptimizelyInfo,
    BestSellersInfo,
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
