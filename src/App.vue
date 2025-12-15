<template>
  <div id="ephemeride" class="mw-body">
    <EphemerideHeader :date-iso="dateISO" />
    <DateSelector :value="dateISO" @update:date="handleChangeDate" />

    <ErrorMessage v-if="erreur" :message="erreur" />
    <LoadingIndicator v-else-if="chargement" />
    <template v-else>
      <EventSection
        v-if="donnees.events?.length"
        title="Événements"
        :items="donnees.events"
      />
      <EventSection
        v-if="donnees.births?.length"
        title="Naissances"
        :items="donnees.births"
      />
      <EventSection
        v-if="donnees.deaths?.length"
        title="Décès"
        :items="donnees.deaths"
      />
      <EmptyState v-if="vide" />
    </template>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import EphemerideHeader from './components/EphemerideHeader.vue'
import DateSelector from './components/DateSelector.vue'
import EventSection from './components/EventSection.vue'
import ErrorMessage from './components/ErrorMessage.vue'
import LoadingIndicator from './components/LoadingIndicator.vue'
import EmptyState from './components/EmptyState.vue'

const dateISO = ref('')
const donnees = ref({})
const chargement = ref(false)
const erreur = ref(null)

const moisNoms = [
  'janvier', 'février', 'mars', 'avril', 'mai', 'juin',
  'juillet', 'août', 'septembre', 'octobre', 'novembre', 'décembre'
]

const vide = computed(() => {
  const d = donnees.value
  return !d.events?.length && !d.births?.length && !d.deaths?.length
})

const chargerDonnees = async (mm, dd) => {
  const daysInMonth = new Date(2023, mm, 0).getDate()
  if (dd < 1 || dd > daysInMonth) {
    erreur.value = 'Date invalide (ex: 31 avril)'
    donnees.value = {}
    return
  }

  chargement.value = true
  erreur.value = null
  donnees.value = {}

  try {
    const types = ['events', 'births', 'deaths']
    const result = {}

    for (const t of types) {
      // 🔧 URL corrigée : plus d'espaces
      const url = `https://fr.wikipedia.org/api/rest_v1/feed/onthisday/${t}/${mm}/${dd}`
      
      // 🔧 Suppression du User-Agent (bloqué par Firefox)
      const response = await fetch(url)

      if (response.ok) {
        const data = await response.json()
        const items = data[t] || []
        const cleaned = items
          .map(item => {
            const pages = (item.pages || [])
              .map(page => ({
                title: page.title || '',
                thumbnail: page.thumbnail?.source || null,
                // 🔧 URL corrigée
                wiki_url: page.title ? `https://fr.wikipedia.org/wiki/${encodeURIComponent(page.title)}` : null
              }))
              .filter(p => p.title)

            return {
              year: item.year || null,
              text: item.text || '',
              pages: pages
            }
          })
          .filter(item => item.year !== null)

        result[t] = cleaned
      } else {
        result[t] = []
      }
    }

    donnees.value = result
  } catch (e) {
    erreur.value = 'Impossible de charger les données. Vérifie ta connexion.'
    console.error(e)
  } finally {
    chargement.value = false
  }
}

const handleChangeDate = (newDate) => {
  const d = new Date(newDate)
  const mm = d.getMonth() + 1
  const dd = d.getDate()
  dateISO.value = newDate
  chargerDonnees(mm, dd)
}

onMounted(() => {
  const today = new Date()
  const mm = today.getMonth() + 1
  const dd = today.getDate()
  dateISO.value = today.toISOString().split('T')[0]
  chargerDonnees(mm, dd)
})
</script>

<style>
#ephemeride {
  max-width: 850px;
  margin: 0 auto;
  padding: 1em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', sans-serif;
  font-size: 14pt;
  line-height: 1.5;
  color: #202122;
  background: white;
}
</style>