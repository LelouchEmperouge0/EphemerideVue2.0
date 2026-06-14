<template>
  <div class="skeleton-wrapper" role="status" aria-label="Chargement des événements historiques…">
    <div class="skeleton-section" v-for="s in 3" :key="s">
      <div class="skeleton-section-header">
        <div class="skeleton-block skeleton-icon"></div>
        <div class="skeleton-block skeleton-label"></div>
      </div>
      <div class="skeleton-cards">
        <div class="skeleton-card" v-for="c in 4" :key="c">
          <div class="skeleton-year-col">
            <div class="skeleton-block skeleton-year"></div>
          </div>
          <div class="skeleton-divider"></div>
          <div class="skeleton-body">
            <div class="skeleton-block skeleton-thumb"></div>
            <div class="skeleton-lines">
              <div class="skeleton-block skeleton-line skeleton-line--long"></div>
              <div class="skeleton-block skeleton-line skeleton-line--medium"></div>
              <div class="skeleton-block skeleton-line skeleton-line--short"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <span class="sr-only">Chargement en cours…</span>
  </div>
</template>

<style scoped>
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.45; }
}

.skeleton-wrapper {
  display: flex;
  flex-direction: column;
  gap: var(--space-10);
}

.skeleton-block {
  background: var(--color-border);
  border-radius: var(--radius-sm);
  animation: pulse 1.6s ease-in-out infinite;
}

.skeleton-section-header {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  margin-bottom: var(--space-5);
}

.skeleton-icon {
  width: 18px;
  height: 18px;
  border-radius: var(--radius-sm);
  animation-delay: 0ms;
}

.skeleton-label {
  width: 110px;
  height: 12px;
  animation-delay: 80ms;
}

.skeleton-cards {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}

.skeleton-card {
  display: flex;
  align-items: stretch;
  background: var(--color-bg-card);
  border-radius: var(--radius-md);
  border: 1px solid var(--color-border);
  border-left: 3px solid var(--color-border);
  overflow: hidden;
}

.skeleton-year-col {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-4) var(--space-5);
  min-width: 88px;
}

.skeleton-year {
  width: 52px;
  height: 24px;
  animation-delay: 120ms;
}

.skeleton-divider {
  width: 1px;
  background: var(--color-border);
  align-self: stretch;
  flex-shrink: 0;
}

.skeleton-body {
  display: flex;
  align-items: flex-start;
  gap: var(--space-4);
  padding: var(--space-4) var(--space-5);
  flex: 1;
}

.skeleton-thumb {
  width: 72px;
  height: 72px;
  flex-shrink: 0;
  animation-delay: 160ms;
}

.skeleton-lines {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  padding-top: 4px;
}

.skeleton-line {
  height: 14px;
}

.skeleton-line--long {
  width: 92%;
  animation-delay: 200ms;
}

.skeleton-line--medium {
  width: 76%;
  animation-delay: 240ms;
}

.skeleton-line--short {
  width: 48%;
  animation-delay: 280ms;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

@media (max-width: 520px) {
  .skeleton-year-col {
    min-width: unset;
    padding: var(--space-3) var(--space-4);
  }
}
</style>
