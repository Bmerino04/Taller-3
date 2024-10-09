<template>
  <div>
    <h1>Lista de países</h1>
    <button @click="toggleLista">{{ mostrarLista ? 'Ocultar' : 'Mostrar' }} países</button>
    <ul v-if="mostrarLista">
      <li v-for="(dato, index) in datos" :key="index">
        <h2>{{ dato.country }} ({{ dato.code }})</h2>
        <div v-for="(poblacion, i) in dato.populationCounts" :key="i">
          <span>Población {{ i + 1 }}: {{ poblacion.value }}</span>
        </div>
      </li>
    </ul>

    <h1>Lista de recomendaciones de Anime</h1>
    <button @click="toggleAnimeLista">{{ mostrarAnimeLista ? 'Ocultar' : 'Mostrar' }} animes</button>
    <ul v-if="mostrarAnimeLista">
      <li v-for="(anime, index) in animes" :key="index">
        <div>
          <h2>{{ anime.entry[0].title }} & {{ anime.entry[1]?.title || 'No disponible' }}</h2>
          <p>{{ anime.content }}</p>
          <p>Recomendado por: {{ anime.user.username }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const datos = ref([])
const mostrarLista = ref(false)

const animes = ref([])
const mostrarAnimeLista = ref(false)

const toggleLista = () => {
  mostrarLista.value = !mostrarLista.value
}

const toggleAnimeLista = () => {
  mostrarAnimeLista.value = !mostrarAnimeLista.value
}

onMounted(async () => {
  try {
    const res = await fetch('https://countriesnow.space/api/v0.1/countries/population')
    if (!res.ok) {
      throw new Error('Error al cargar países')
    }
    const { data } = await res.json()
    datos.value = data
  } catch (error) {
    console.error('Error al cargar países:', error)
  }

  try {
    const animeRes = await fetch('https://api.jikan.moe/v4/recommendations/anime')
    if (!animeRes.ok) {
      throw new Error('Error al cargar recomendaciones de anime')
    }
    const { data } = await animeRes.json()
    animes.value = data
  } catch (error) {
    console.error('Error al cargar recomendaciones de anime:', error)
  }
})
</script>

