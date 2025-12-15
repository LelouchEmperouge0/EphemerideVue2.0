<template>
  <div class="error">{{ message }}</div>
</template>

<script setup>
defineProps({
  message: { type: String, required: true }
})
</script>

<style scoped>
.error {
  color: #d73a49;
  text-align: center;
  margin: 2em 0;
  font-weight: bold;
}
</style>