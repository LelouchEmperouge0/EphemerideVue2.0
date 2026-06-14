<template>
  <section class="event-section" :class="`section--${type}`">
    <div class="section-header">
      <div class="section-title-row">
        <span class="section-icon" aria-hidden="true">
          <svg v-if="type === 'events'" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5">
            <circle cx="10" cy="10" r="7.5"/>
            <polyline points="10,5.5 10,10 13,12.5"/>
          </svg>
          <svg v-else-if="type === 'births'" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M10 3 C6 3 3 6 3 10 C3 14 6 17 10 17 C14 17 17 14 17 10"/>
            <polyline points="12.5,3 17,3 17,7.5"/>
          </svg>
          <svg v-else viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5">
            <line x1="10" y1="4" x2="10" y2="16"/>
            <line x1="4" y1="10" x2="16" y2="10"/>
            <rect x="4" y="14" width="12" height="2" rx="1"/>
          </svg>
        </span>
        <h2 class="section-title">{{ title }}</h2>
        <span class="section-badge">{{ items.length }}</span>
      </div>
      <div class="section-rule" aria-hidden="true"></div>
    </div>

    <div class="section-cards">
      <EventCard
        v-for="(item, index) in items"
        :key="`${type}-${index}`"
        :item="item"
        :type="type"
      />
    </div>
  </section>
</template>

<script setup>
import EventCard from './EventCard.vue'

defineProps({
  title: { type: String, required: true },
  type: { type: String, required: true },
  items: { type: Array, required: true }
})
</script>

<style scoped>
.event-section {
  margin-bottom: var(--space-10);
}

.section-header {
  margin-bottom: var(--space-5);
}

.section-title-row {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  margin-bottom: var(--space-3);
}

.section-icon {
  display: flex;
  align-items: center;
  flex-shrink: 0;
}

.section-icon svg {
  width: 18px;
  height: 18px;
}

.section--events .section-icon {
  color: var(--color-events);
}

.section--births .section-icon {
  color: var(--color-births);
}

.section--deaths .section-icon {
  color: var(--color-deaths);
}

.section-title {
  font-family: var(--font-ui);
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}

.section-badge {
  font-family: var(--font-ui);
  font-size: 11px;
  font-weight: 600;
  padding: 2px 8px;
  border-radius: var(--radius-full);
  line-height: 1.5;
}

.section--events .section-badge {
  background: var(--color-events-bg);
  color: var(--color-events);
}

.section--births .section-badge {
  background: var(--color-births-bg);
  color: var(--color-births);
}

.section--deaths .section-badge {
  background: var(--color-deaths-bg);
  color: var(--color-deaths);
}

.section-rule {
  height: 1px;
  background: var(--color-border);
}

.section-cards {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}
</style>
