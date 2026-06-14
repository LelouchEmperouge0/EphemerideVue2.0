<template>
  <article class="event-card" :class="`card--${item.type || 'events'}`">
    <div class="card-year-col">
      <span class="card-year">{{ item.year }}</span>
    </div>

    <div class="card-divider" aria-hidden="true"></div>

    <div class="card-body">
      <div class="card-media" v-if="item.pages?.length">
        <img
          v-if="item.pages[0].thumbnail"
          :src="item.pages[0].thumbnail"
          :alt="item.pages[0].title"
          class="card-thumb"
          loading="lazy"
          decoding="async"
        />
      </div>

      <div class="card-content">
        <p class="card-text">{{ item.text }}</p>
        <a
          v-if="item.pages?.[0]?.wiki_url"
          :href="item.pages[0].wiki_url"
          target="_blank"
          rel="noopener noreferrer"
          class="card-link"
          :aria-label="`Lire sur Wikipédia : ${item.pages[0].title}`"
        >
          Lire sur Wikipédia
          <svg viewBox="0 0 12 12" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true">
            <path d="M2 10L10 2M6 2h4v4"/>
          </svg>
        </a>
      </div>
    </div>
  </article>
</template>

<script setup>
defineProps({
  item: { type: Object, required: true },
  type: { type: String, default: 'events' }
})
</script>

<style scoped>
.event-card {
  display: flex;
  align-items: stretch;
  background: var(--color-bg-card);
  border-radius: var(--radius-md);
  border: 1px solid var(--color-border);
  border-left-width: 3px;
  box-shadow: var(--shadow-sm);
  overflow: hidden;
  transition: box-shadow var(--transition-fast), border-color var(--transition-fast), transform var(--transition-fast);
}

.event-card:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-1px);
}

.card--events {
  border-left-color: var(--color-events);
}

.card--births {
  border-left-color: var(--color-births);
}

.card--deaths {
  border-left-color: var(--color-deaths);
}

.card-year-col {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-4) var(--space-5);
  min-width: 88px;
  flex-shrink: 0;
}

.card-year {
  font-family: var(--font-display);
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1;
  white-space: nowrap;
}

.card--events .card-year {
  color: var(--color-events);
}

.card--births .card-year {
  color: var(--color-births);
}

.card--deaths .card-year {
  color: var(--color-deaths);
}

.card-divider {
  width: 1px;
  background: var(--color-border);
  align-self: stretch;
  flex-shrink: 0;
}

.card-body {
  display: flex;
  align-items: flex-start;
  gap: var(--space-4);
  padding: var(--space-4) var(--space-5);
  flex: 1;
  min-width: 0;
}

.card-media {
  flex-shrink: 0;
}

.card-thumb {
  width: 72px;
  height: 72px;
  object-fit: cover;
  border-radius: var(--radius-sm);
  background: var(--color-bg-section);
}

.card-content {
  flex: 1;
  min-width: 0;
}

.card-text {
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 1.55;
  color: var(--color-text);
  margin-bottom: var(--space-3);
}

.card-link {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-family: var(--font-ui);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.02em;
  text-decoration: none;
  transition: color var(--transition-fast), gap var(--transition-fast);
}

.card--events .card-link {
  color: var(--color-events);
}

.card--births .card-link {
  color: var(--color-births);
}

.card--deaths .card-link {
  color: var(--color-deaths);
}

.card-link:hover {
  gap: 8px;
}

.card-link svg {
  width: 10px;
  height: 10px;
  flex-shrink: 0;
}

@media (max-width: 520px) {
  .event-card {
    flex-direction: column;
    border-left-width: 3px;
    border-top-width: 1px;
  }

  .card-year-col {
    padding: var(--space-3) var(--space-4);
    min-width: unset;
    justify-content: flex-start;
    border-bottom: 1px solid var(--color-border);
  }

  .card-year {
    font-size: 1.25rem;
  }

  .card-divider {
    display: none;
  }

  .card-body {
    padding: var(--space-3) var(--space-4);
  }

  .card-thumb {
    width: 56px;
    height: 56px;
  }
}
</style>
