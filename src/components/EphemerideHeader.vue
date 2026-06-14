<template>
  <header class="hero">
    <div class="hero-inner">
      <div class="hero-brand">
        <svg class="hero-ornament" viewBox="0 0 24 12" fill="none" aria-hidden="true">
          <line x1="0" y1="6" x2="8" y2="6" stroke="currentColor" stroke-width="1"/>
          <circle cx="12" cy="6" r="2" fill="currentColor"/>
          <line x1="16" y1="6" x2="24" y2="6" stroke="currentColor" stroke-width="1"/>
        </svg>
        <span class="hero-label">Éphéméride</span>
        <svg class="hero-ornament" viewBox="0 0 24 12" fill="none" aria-hidden="true">
          <line x1="0" y1="6" x2="8" y2="6" stroke="currentColor" stroke-width="1"/>
          <circle cx="12" cy="6" r="2" fill="currentColor"/>
          <line x1="16" y1="6" x2="24" y2="6" stroke="currentColor" stroke-width="1"/>
        </svg>
      </div>

      <h1 class="hero-date">{{ jourFormate }}</h1>
      <p class="hero-year">{{ anneeFormatee }}</p>

      <div class="hero-picker">
        <button type="button" class="picker-label" @click="openPicker" aria-haspopup="dialog">
          <svg viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true">
            <rect x="3" y="4" width="14" height="14" rx="2"/>
            <line x1="7" y1="2" x2="7" y2="6"/>
            <line x1="13" y1="2" x2="13" y2="6"/>
            <line x1="3" y1="9" x2="17" y2="9"/>
          </svg>
          Changer la date
        </button>
        <input
          ref="pickerRef"
          type="date"
          :value="isoDate"
          :max="todayISO"
          @change="handleChange"
          class="picker-input"
          aria-label="Sélectionner une date"
          tabindex="-1"
        />
      </div>
    </div>
  </header>
</template>

<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  isoDate: { type: String, required: true }
})

const emit = defineEmits(['change-date'])

const MOIS = [
  'janvier', 'février', 'mars', 'avril', 'mai', 'juin',
  'juillet', 'août', 'septembre', 'octobre', 'novembre', 'décembre'
]

const JOURS = [
  'dimanche', 'lundi', 'mardi', 'mercredi', 'jeudi', 'vendredi', 'samedi'
]

const parseDate = (iso) => {
  const [yyyy, mm, dd] = iso.split('-').map(Number)
  return new Date(yyyy, mm - 1, dd)
}

const jourFormate = computed(() => {
  if (!props.isoDate) return ''
  const d = parseDate(props.isoDate)
  return `${d.getDate()} ${MOIS[d.getMonth()]}`
})

const anneeFormatee = computed(() => {
  if (!props.isoDate) return ''
  const d = parseDate(props.isoDate)
  return `${JOURS[d.getDay()]} ${d.getFullYear()}`
})

const todayISO = computed(() => {
  const t = new Date()
  const yyyy = t.getFullYear()
  const mm = String(t.getMonth() + 1).padStart(2, '0')
  const dd = String(t.getDate()).padStart(2, '0')
  return `${yyyy}-${mm}-${dd}`
})

const pickerRef = ref(null)

const openPicker = () => {
  if (!pickerRef.value) return
  if (typeof pickerRef.value.showPicker === 'function') {
    pickerRef.value.showPicker()
  } else {
    pickerRef.value.click()
  }
}

const handleChange = (e) => {
  emit('change-date', e.target.value)
}
</script>

<style scoped>
.hero {
  background: var(--color-hero-bg);
  color: var(--color-hero-text);
  padding: var(--space-12) var(--space-4) var(--space-10);
  text-align: center;
}

.hero-inner {
  max-width: var(--content-width);
  margin: 0 auto;
}

.hero-brand {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-3);
  color: var(--color-accent);
  margin-bottom: var(--space-6);
}

.hero-ornament {
  width: 48px;
  height: 12px;
  opacity: 0.8;
}

.hero-label {
  font-family: var(--font-ui);
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--color-accent);
}

.hero-date {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 8vw, 4.5rem);
  font-weight: 700;
  line-height: 1.1;
  color: var(--color-hero-text);
  margin-bottom: var(--space-2);
  letter-spacing: -0.01em;
}

.hero-year {
  font-family: var(--font-ui);
  font-size: 14px;
  font-weight: 400;
  color: var(--color-hero-muted);
  letter-spacing: 0.04em;
  margin-bottom: var(--space-8);
  text-transform: capitalize;
}

.hero-picker {
  display: inline-flex;
  align-items: center;
  position: relative;
}

.picker-label {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.18);
  color: var(--color-hero-text);
  font-family: var(--font-ui);
  font-size: 14px;
  font-weight: 500;
  padding: var(--space-2) var(--space-5);
  border-radius: var(--radius-full);
  cursor: pointer;
  transition: background var(--transition-fast), border-color var(--transition-fast);
  appearance: none;
  -webkit-appearance: none;
}

.picker-label svg {
  width: 16px;
  height: 16px;
  flex-shrink: 0;
  opacity: 0.85;
}

.picker-label:hover {
  background: rgba(255, 255, 255, 0.16);
  border-color: rgba(255, 255, 255, 0.3);
}

.picker-input {
  position: absolute;
  width: 1px;
  height: 1px;
  opacity: 0;
  pointer-events: none;
  overflow: hidden;
}
</style>
