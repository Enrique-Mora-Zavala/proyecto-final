<script setup>
import { ref, onMounted } from 'vue'
import api from '../../services/api'

const docentes = ref([])

const emit = defineEmits(['editar-docente'])

const cargarDocentes = async () => {
  const response = await api.get('/docentes')
  docentes.value = response.data
}

const eliminarDocente = async (id) => {
  const confirmar = confirm('¿Seguro que deseas eliminar este docente?')

  if (confirmar) {
    await api.delete(`/docentes/${id}`)
    cargarDocentes()
  }
}

const seleccionarDocente = (docente) => {
  emit('editar-docente', docente)
}

onMounted(() => {
  cargarDocentes()
})
</script>

<template>
  <div>
    <h2>Lista de docentes</h2>

    <p v-if="docentes.length === 0">No hay docentes registrados.</p>

    <table v-else border="1">
      <thead>
        <tr>
          <th>Número de empleado</th>
          <th>Nombre completo</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="docente in docentes" :key="docente.id">
          <td>{{ docente.id }}</td>
          <td>{{ docente.nombreCompleto }}</td>
          <td>
            <button @click="seleccionarDocente(docente)">✏️</button>
            <button @click="eliminarDocente(docente.id)">🗑️</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
