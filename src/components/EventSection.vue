<template>
  <h2>{{ title }}</h2>
  <div class="section-cards">
    <EventCard v-for="(item, index) in items" :key="`${title}-${index}`" :item="item" />
  </div>
</template>

<script setup>
import EventCard from './EventCard.vue'

defineProps({
  title: { type: String, required: true },
  items: { type: Array, required: true }
})
</script>

<style scoped>
h2 {
  font-size: 16pt;
  font-weight: bold;
  margin: 1.2em 0 0.5em;
  background-color: #eaecf0;
  padding: 0.2em 0.4em;
  border-radius: 2px;
  color: #000;
}
.section-cards {
  margin: 1em 0 2em;
}
</style>