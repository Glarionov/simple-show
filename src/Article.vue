<template>
  <article class="post container mt-4" >
    <header class="post__header row">
      <div class="col-8 post__user-name">
        <span v-if="Object.prototype.hasOwnProperty.call(item, 'user') && Object.prototype.hasOwnProperty.call(item.user, 'name')" >
          {{ item.user.name }}
        </span>
      </div>
      <time class="col-4 post__date">
        {{convertedDate}}
      </time>
    </header>

    <div v-if="Object.prototype.hasOwnProperty.call(item, 'text')" class="row">
      <p class="col-12 p-2 post__text">
        {{ item.text }}<br/>
      </p>
    </div>
  </article>
</template>

<script>
export default {
  name: "Article",
  props: {
    item: {
      type: Object
    }
  },
  computed: {
    convertedDate() {
      if (!Object.prototype.hasOwnProperty.call(this.item, 'created_at')) {
        return '';
      }

      let d = new Date(this.item.created_at);
      return d.toLocaleDateString() + ' ' + d.toLocaleTimeString();
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