<template>
  <h1 class="firstHeading">Éphéméride du {{ jourFrancais }}</h1>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  isoDate: { type: String, required: true }
})

const moisNoms = [
  'janvier', 'février', 'mars', 'avril', 'mai', 'juin',
  'juillet', 'août', 'septembre', 'octobre', 'novembre', 'décembre'
]

const jourFrancais = computed(() => {
  const d = new Date(props.isoDate || new Date().toISOString().split('T')[0])
  return `${d.getDate()} ${moisNoms[d.getMonth()]}`
})
</script>

<style scoped>
.firstHeading {
  font-size: 22pt;
  font-weight: bold;
  margin: 0.5em 0;
  padding-bottom: 0.17em;
  border-bottom: 1px solid #a2a9b1;
  color: #000;
}
</style>