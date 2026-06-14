<template>
  <div id="app">
    <EphemerideHeader
      :iso-date="dateISO"
      @change-date="handleChangeDate"
    />

    <main class="main-content">
      <div class="content-container">
        <Transition name="fade" mode="out-in">
          <ErrorMessage
            v-if="erreur"
            :message="erreur"
            @retry="retryLoad"
            key="error"
          />
          <LoadingIndicator v-else-if="chargement" key="loading" />
          <div v-else key="content">
            <EventSection
              v-if="donnees.events?.length"
              title="Événements"
              type="events"
              :items="donnees.events"
            />
            <EventSection
              v-if="donnees.births?.length"
              title="Naissances"
              type="births"
              :items="donnees.births"
            />
            <EventSection
              v-if="donnees.deaths?.length"
              title="Décès"
              type="deaths"
              :items="donnees.deaths"
            />
            <EmptyState v-if="vide" />
          </div>
        </Transition>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import EphemerideHeader from './components/EphemerideHeader.vue'
import EventSection from './components/EventSection.vue'
import ErrorMessage from './components/ErrorMessage.vue'
import LoadingIndicator from './components/LoadingIndicator.vue'
import EmptyState from './components/EmptyState.vue'

const dateISO = ref('')
const donnees = ref({})
const chargement = ref(false)
const erreur = ref(null)
let abortController = null

const vide = computed(() => {
  const d = donnees.value
  return !d.events?.length && !d.births?.length && !d.deaths?.length
})

const parseISODate = (iso) => {
  const [, mm, dd] = iso.split('-').map(Number)
  return { mm, dd }
}

const transformItem = (item, type) => {
  const pages = (item.pages || [])
    .map(page => ({
      title: page.title || '',
      thumbnail: page.thumbnail?.source || null,
      wiki_url: page.title
        ? `https://fr.wikipedia.org/wiki/${encodeURIComponent(page.title)}`
        : null
    }))
    .filter(p => p.title)

  return {
    year: item.year ?? null,
    text: item.text || '',
    pages,
    type
  }
}

const chargerDonnees = async (mm, dd) => {
  if (abortController) abortController.abort()
  abortController = new AbortController()
  const { signal } = abortController

  const daysInMonth = new Date(2023, mm, 0).getDate()
  if (dd < 1 || dd > daysInMonth) {
    erreur.value = "Date invalide (ex\u00a0: le 31 avril n'existe pas)."
    donnees.value = {}
    return
  }

  chargement.value = true
  erreur.value = null
  donnees.value = {}

  try {
    const types = ['events', 'births', 'deaths']
    const fetches = types.map(t =>
      fetch(
        `https://fr.wikipedia.org/api/rest_v1/feed/onthisday/${t}/${mm}/${dd}`,
        { signal }
      ).then(r => (r.ok ? r.json() : Promise.resolve({ [t]: [] })))
    )

    const results = await Promise.allSettled(fetches)
    const result = {}

    types.forEach((t, i) => {
      if (results[i].status === 'fulfilled') {
        result[t] = (results[i].value[t] || [])
          .filter(item => item.year != null)
          .map(item => transformItem(item, t))
      } else {
        result[t] = []
      }
    })

    donnees.value = result
  } catch (e) {
    if (e.name !== 'AbortError') {
      erreur.value = 'Impossible de charger les données. Vérifiez votre connexion.'
      console.error(e)
    }
  } finally {
    chargement.value = false
  }
}

const handleChangeDate = (newDate) => {
  dateISO.value = newDate
  const { mm, dd } = parseISODate(newDate)
  chargerDonnees(mm, dd)
}

const retryLoad = () => {
  if (dateISO.value) {
    const { mm, dd } = parseISODate(dateISO.value)
    chargerDonnees(mm, dd)
  }
}

onMounted(() => {
  const today = new Date()
  const yyyy = today.getFullYear()
  const mm = String(today.getMonth() + 1).padStart(2, '0')
  const dd = String(today.getDate()).padStart(2, '0')
  dateISO.value = `${yyyy}-${mm}-${dd}`
  chargerDonnees(Number(mm), Number(dd))
})
</script>

<style>
.main-content {
  flex: 1;
  padding: var(--space-8) var(--space-4);
}

.content-container {
  max-width: var(--content-width);
  margin: 0 auto;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity var(--transition-base);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
