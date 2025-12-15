<template>
  <div class="event-card">
    <img
  v-if="item.pages?.[0]?.thumbnail"
  :src="item.pages[0].thumbnail"
  class="card-thumb"
  :alt="item.pages[0].title || 'Image par défaut'"
/>
<img
  v-else
  src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Wikipedia-logo-v2.svg/120px-Wikipedia-logo-v2.svg.png"
  class="card-thumb"
  alt="Pas d'image disponible"
/>
    <div class="card-content">
      <div class="card-year">{{ item.year }}</div>
      <div class="card-text">{{ item.text }}</div>
      <a
  v-if="item.pages?.[0]?.wiki_url"
  :href="item.pages[0].wiki_url"
  target="_blank"
  class="card-link"
>
  Lire sur Wikipédia →
</a>
<span v-else class="card-link-disabled">
  Aucun article Wikipédia associé
</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  item: { type: Object, required: true }
})
</script>

<style scoped>
.event-card {
  display: flex;
  gap: 1em;
  padding: 1em;
  margin-bottom: 1em;
  border: 1px solid #eaecf0;
  border-radius: 6px;
  background: #f8f9fa;
  transition: border-color 0.2s;
}
.event-card:hover {
  border-color: #a2a9b1;
}
.card-thumb {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 4px;
  flex-shrink: 0;
}
.card-content {
  flex: 1;
}
.card-year {
  font-weight: bold;
  color: #0645ad;
  margin-bottom: 0.3em;
  font-size: 15pt;
}
.card-text {
  margin-bottom: 0.5em;
  line-height: 1.5;
}
.card-link {
  color: #0645ad;
  text-decoration: none;
  font-size: 0.95em;
  font-weight: bold;
}
.card-link:hover {
  text-decoration: underline;
}
.card-link-disabled {
  color: #999;
  font-size: 0.95em;
  font-weight: bold;
  cursor: not-allowed;
}



</style>