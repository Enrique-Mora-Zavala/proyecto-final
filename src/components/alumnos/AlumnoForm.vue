<script setup>
import { ref, onMounted } from 'vue'
import api from '../../services/api'

const emit = defineEmits(['alumno-agregado'])

const carreras = ref([])

const alumno = ref({
  nombreCompleto: '',
  numeroControl: '',
  carreraId: '',
  semestre: '',
})

const cargarCarreras = async () => {
  const response = await api.get('/carreras')
  carreras.value = response.data
}

const guardarAlumno = async () => {
  await api.post('/alumnos', {
    ...alumno.value,
    carreraId: Number(alumno.value.carreraId),
    semestre: Number(alumno.value.semestre),
  })

  alumno.value = {
    nombreCompleto: '',
    numeroControl: '',
    carreraId: '',
    semestre: '',
  }

  emit('alumno-agregado')
}

onMounted(() => {
  cargarCarreras()
})
</script>

<template>
  <form @submit.prevent="guardarAlumno">
    <h2>Registrar alumno</h2>

    <div>
      <label>Nombre completo:</label>
      <input v-model="alumno.nombreCompleto" type="text" required />
    </div>

    <div>
      <label>Número de control:</label>
      <input v-model="alumno.numeroControl" type="text" required />
    </div>

    <div>
      <label>Carrera:</label>
      <select v-model="alumno.carreraId" required>
        <option value="">Seleccione una carrera</option>
        <option v-for="carrera in carreras" :key="carrera.id" :value="carrera.id">
          {{ carrera.nombre }}
        </option>
      </select>
    </div>

    <div>
      <label>Semestre:</label>
      <input v-model="alumno.semestre" type="number" min="1" max="12" required />
    </div>

    <button type="submit">Guardar alumno</button>
  </form>
</template>
