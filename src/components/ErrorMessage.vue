<template>
  <div class="error-state" role="alert">
    <div class="error-icon" aria-hidden="true">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
        <circle cx="12" cy="12" r="9.5"/>
        <line x1="12" y1="7" x2="12" y2="13"/>
        <circle cx="12" cy="16.5" r="0.75" fill="currentColor" stroke="none"/>
      </svg>
    </div>
    <div class="error-content">
      <p class="error-title">Une erreur est survenue</p>
      <p class="error-message">{{ message }}</p>
    </div>
    <button
      v-if="showRetry"
      class="error-retry"
      type="button"
      @click="$emit('retry')"
    >
      <svg viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true">
        <path d="M4 10a6 6 0 0 1 6-6 6 6 0 0 1 4.24 1.76"/>
        <polyline points="14.5,2 16,5.76 12.24,7.25"/>
      </svg>
      Réessayer
    </button>
  </div>
</template>

<script setup>
defineProps({
  message: { type: String, required: true },
  showRetry: { type: Boolean, default: true }
})

defineEmits(['retry'])
</script>

<style scoped>
.error-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-4);
  padding: var(--space-10) var(--space-4);
  text-align: center;
}

.error-icon {
  width: 48px;
  height: 48px;
  color: #C0392B;
  opacity: 0.85;
}

.error-icon svg {
  width: 100%;
  height: 100%;
}

.error-content {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.error-title {
  font-family: var(--font-ui);
  font-size: 15px;
  font-weight: 600;
  color: var(--color-text);
}

.error-message {
  font-family: var(--font-body);
  font-size: 15px;
  color: var(--color-text-muted);
  max-width: 360px;
}

.error-retry {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-ui);
  font-size: 13px;
  font-weight: 600;
  color: var(--color-accent);
  padding: var(--space-2) var(--space-5);
  border: 1.5px solid var(--color-accent);
  border-radius: var(--radius-full);
  background: transparent;
  cursor: pointer;
  transition: background var(--transition-fast), color var(--transition-fast);
}

.error-retry:hover {
  background: var(--color-accent-dim);
}

.error-retry:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 3px;
}

.error-retry svg {
  width: 14px;
  height: 14px;
}
</style>
