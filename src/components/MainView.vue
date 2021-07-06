<template>
  <section class="container simple-list-main">
    <article class="post container mt-4" v-for="(item) in items" :key="item.id">
      <header class="post__header row">
        <div class="col-8 post__user-name">
          {{ item.user.name }}
        </div>
        <time class="col-4 post__date">
          {{ item.created_at|convertDate }}
        </time>
      </header>

      <div class="row">
        <p class="col-12 p-2 post__text">
          {{ item.text }}<br/>
        </p>
      </div>
    </article>
  </section>
</template>

<script>
export default {
  name: "MainView",
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
        items: []
      }
    },
  mounted() {
    this.loadData();
    setInterval(this.loadData, this.updateInterval);
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
    }
  }
}
</script>

<style scoped>
  .post {
    background: #dcdcff;
    color: #402701;
    font-size: 19px;
    border: 2px solid #1b002d;
  }

  .post .post__header {
    background: #a5a5ff;
  }

  .post .post__date {
    font-size: 15px;
  }

  .post .post__user-name {
    text-align: left;
    font-weight: 600;
  }
</style>
