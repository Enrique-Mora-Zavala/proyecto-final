<script setup>
import { ref, onMounted } from 'vue'
import api from '../../services/api'

const emit = defineEmits(['editar-proyecto'])

const proyectos = ref([])
const docentes = ref([])

const cargarProyectos = async () => {
  const response = await api.get('/proyectos')
  proyectos.value = response.data
}

const cargarDocentes = async () => {
  const response = await api.get('/docentes')
  docentes.value = response.data
}

const obtenerDocente = (id) => {
  const docente = docentes.value.find((d) => d.id === id)
  return docente ? docente.nombreCompleto : 'Sin docente'
}

const seleccionarProyecto = (proyecto) => {
  emit('editar-proyecto', proyecto)
}

const eliminarProyecto = async (id) => {
  const confirmar = confirm('¿Seguro que deseas eliminar este proyecto?')

  if (confirmar) {
    await api.delete(`/proyectos/${id}`)
    cargarProyectos()
  }
}

onMounted(() => {
  cargarProyectos()
  cargarDocentes()
})
</script>

<template>
  <div>
    <h2>Lista de proyectos</h2>

    <p v-if="proyectos.length === 0">No hay proyectos registrados.</p>

    <table v-else border="1">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Descripción</th>
          <th>Fecha inicio</th>
          <th>Docente</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="proyecto in proyectos" :key="proyecto.id">
          <td>{{ proyecto.id }}</td>
          <td>{{ proyecto.nombre }}</td>
          <td>{{ proyecto.descripcion }}</td>
          <td>{{ proyecto.fechaInicio }}</td>
          <td>{{ obtenerDocente(proyecto.docenteId) }}</td>
          <td>
            <button @click="seleccionarProyecto(proyecto)">✏️</button>
            <button @click="eliminarProyecto(proyecto.id)">🗑️</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
