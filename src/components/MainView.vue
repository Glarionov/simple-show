<template>
  <section class="container simple-list-main">
    <Article v-for="(item) in items" :key="item.id" :item="item" />
  </section>
</template>

<script>
import Article from "../Article";
export default {
  name: "MainView",
  components: {Article},
  props: {
    postLimit: {
      type: Number,
      default: 5
    },
    updateInterval: {
      type: Number,
      default: 5000
    },
    postUrl: {
      type: String,
      default: 'http://api.massrelevance.com/MassRelDemo/kindle.json'
    }
  },
  data: function () {
      return {
        items: [],
        realUpdateInterval: 1000,
        updateIntervalMin: 1000,
        intervalObject: null
      }
    },
  mounted() {
    this.setNewUpdateInterval();
  },
  filters: {
    convertDate(inputFormat) {
      function pad(s) { return (s < 10) ? '0' + s : s; }
      let d = new Date(inputFormat);
      let hoursAndMinutes = ' ' + pad(d.getHours()) + ':' + pad(d.getMinutes());
      return [pad(d.getDate()), pad(d.getMonth()+1), d.getFullYear()].join('/') + hoursAndMinutes;
    },
  },
  methods: {
    loadData() {
      fetch(this.postUrl)
          .then((response) => {
            if(response.ok) {
              return response.json();
            }
            throw new Error('Network response was not ok');
          })
          .then((json) => {
            this.items = json.slice(0, this.postLimit);
          })
          .catch((error) => {
            console.log(error);
          });
    },
    setNewUpdateInterval() {
      this.realUpdateInterval = Math.max(this.updateInterval, this.updateIntervalMin);

      if (this.intervalObject) {
        clearInterval(this.intervalObject);
      }

      this.intervalObject = setInterval(this.loadData, this.realUpdateInterval);
    },
  },
  watch: {
    updateInterval: function () {
      this.setNewUpdateInterval();
    }
  },
  beforeDestroy() {
    clearInterval(this.intervalObject);
  }
}
</script>

<style scoped>

</style>
