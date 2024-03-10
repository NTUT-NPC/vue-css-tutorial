<script setup>
defineProps({
  author: String,
  date: Date,
  content: String,
})
</script>

<template>
  <header>
    <h3 class="comment-author">{{ author }}</h3>
    &middot;
    <time>{{ date.toLocaleString() }}</time>
  </header>
  <p>{{ content }}</p>
</template>

<style scoped>
* {
  font-family: sans-serif;
}

.comment-author {
  display: inline;
  font-weight: bold;
}
</style>