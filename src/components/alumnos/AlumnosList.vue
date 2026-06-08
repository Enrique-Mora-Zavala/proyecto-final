<script setup>
import { ref, onMounted } from 'vue'
import api from '../../services/api'

const alumnos = ref([])
const carreras = ref([])
const emit = defineEmits(['editar-alumno'])

const cargarAlumnos = async () => {
  const response = await api.get('/alumnos')
  alumnos.value = response.data
}

const cargarCarreras = async () => {
  const response = await api.get('/carreras')
  carreras.value = response.data
}

const obtenerCarrera = (id) => {
  const carrera = carreras.value.find((c) => c.id === id)
  return carrera ? carrera.nombre : 'Sin carrera'
}

const eliminarAlumno = async (id) => {
  const confirmar = confirm('¿Seguro que deseas eliminar este alumno?')

  if (confirmar) {
    await api.delete(`/alumnos/${id}`)
    cargarAlumnos()
  }
}

const seleccionarAlumno = (alumno) => {
  emit('editar-alumno', alumno)
}

cargarCarreras()

onMounted(() => {
  cargarAlumnos()
})
</script>

<template>
  <div>
    <h2>Lista de alumnos</h2>

    <p v-if="alumnos.length === 0">No hay alumnos registrados.</p>

    <table v-else border="1">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre completo</th>
          <th>Número de control</th>
          <th>Carrera</th>
          <th>Semestre</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="alumno in alumnos" :key="alumno.id">
          <td>{{ alumno.id }}</td>
          <td>{{ alumno.nombreCompleto }}</td>
          <td>{{ alumno.numeroControl }}</td>
          <td>{{ obtenerCarrera(alumno.carreraId) }}</td>
          <td>{{ alumno.semestre }}</td>
          <td>
            <button @click="seleccionarAlumno(alumno)">Editar</button>
            <button @click="eliminarAlumno(alumno.id)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
