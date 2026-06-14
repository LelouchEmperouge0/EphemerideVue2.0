<template>
  <div class="empty-state">
    <div class="empty-icon" aria-hidden="true">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.2">
        <rect x="3" y="4" width="18" height="16" rx="2"/>
        <line x1="3" y1="9" x2="21" y2="9"/>
        <line x1="8" y1="2" x2="8" y2="6"/>
        <line x1="16" y1="2" x2="16" y2="6"/>
        <line x1="8" y1="14" x2="10" y2="14"/>
        <line x1="8" y1="17" x2="12" y2="17"/>
      </svg>
    </div>
    <p class="empty-title">Aucun événement trouvé</p>
    <p class="empty-message">Il n'y a pas d'événements historiques répertoriés pour cette date.<br>Essayez une autre date.</p>
  </div>
</template>

<style scoped>
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-4);
  padding: var(--space-12) var(--space-4);
  text-align: center;
}

.empty-icon {
  width: 52px;
  height: 52px;
  color: var(--color-text-light);
}

.empty-icon svg {
  width: 100%;
  height: 100%;
}

.empty-title {
  font-family: var(--font-display);
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--color-text);
}

.empty-message {
  font-family: var(--font-body);
  font-size: 15px;
  color: var(--color-text-muted);
  line-height: 1.6;
  max-width: 340px;
}
</style>
