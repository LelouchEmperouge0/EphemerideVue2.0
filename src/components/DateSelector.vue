<template>
  <div class="date-selector">
    <input type="date" :value="value" @change="handleChange" />
  </div>
</template>

<script setup>
const props = defineProps({
  value: { type: String, required: true }
})
const emit = defineEmits(['update:date'])

const handleChange = (e) => {
  emit('update:date', e.target.value)
}
</script>

<style scoped>
.date-selector {
  text-align: center;
  margin: 1em 0;
}
input[type='date'] {
  padding: 0.3em 0.5em;
  font-size: 14pt;
  border: 1px solid #a2a9b1;
  border-radius: 2px;
  background: white;
}
</style>