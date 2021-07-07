<template>
  <div id="app">
    <MainView v-if="!stop" :post-limit=2 :post-url="'http://api.massrelevance.com/MassRelDemo/kindle.json'" :update-interval="this.updateInterval" />
  </div>
</template>

<script>
import MainView from './components/MainView.vue'

export default {
  name: 'App',
  components: {
    MainView
  },
  data: function () {
    return {
      stop: false,
      updateInterval: 1000
    };
  },
  mounted() {
    setTimeout(() => {this.updateInterval = 5000;}, 5000);
    setTimeout(() => {this.stop = true;}, 15000);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
