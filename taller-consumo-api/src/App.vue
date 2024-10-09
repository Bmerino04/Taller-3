<template>
  <div>
    <h1>Lista de países</h1>
    <button @click="toggleLista">{{ mostrarLista ? 'Ocultar' : 'Mostrar' }} paises</button>
    <ul v-if="mostrarLista">
      <li v-for="(dato, index) in datos" :key="index">
        <h2>{{ dato.country }} ({{ dato.code }})</h2>
        <div v-for="(poblacion, i) in dato.populationCounts" :key="i">
          <span>Población {{ i + 1 }}: {{ poblacion.value }}</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const datos = ref([])
const mostrarLista = ref(false)

const toggleLista = () => {
  mostrarLista.value = !mostrarLista.value
}

onMounted(async () => {
  try {
    const res = await fetch('https://countriesnow.space/api/v0.1/countries/population')
    if (!res.ok) {
      throw new Error('Error al cargar')
    }
    const { data } = await res.json()
    datos.value = data
  } catch (error) {
    console.error('Error:', error)
  }
})
</script>
