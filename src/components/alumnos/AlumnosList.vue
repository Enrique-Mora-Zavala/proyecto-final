<script setup>
import { ref, onMounted } from 'vue'
import api from '../../services/api'

const alumnos = ref([])

const cargarAlumnos = async () => {
  const response = await api.get('/alumnos')
  alumnos.value = response.data
}

onMounted(() => {
  cargarAlumnos()
})
</script>

<template>
  <div>
    <h2>Lista de alumnos</h2>

    <p v-if="alumnos.length === 0">No hay alumnos registrados.</p>

    <ul v-else>
      <li v-for="alumno in alumnos" :key="alumno.id">
        {{ alumno.nombreCompleto }} - {{ alumno.numeroControl }}
      </li>
    </ul>
  </div>
</template>
