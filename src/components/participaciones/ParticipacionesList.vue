<script setup>
import { ref, onMounted } from 'vue'
import api from '../../services/api'

const participaciones = ref([])
const alumnos = ref([])
const proyectos = ref([])
const tiposParticipacion = ref([])
const emit = defineEmits(['editar-participacion'])

const cargarDatos = async () => {
  const resParticipaciones = await api.get('/participaciones')
  const resAlumnos = await api.get('/alumnos')
  const resProyectos = await api.get('/proyectos')
  const resTipos = await api.get('/tiposParticipacion')

  participaciones.value = resParticipaciones.data
  alumnos.value = resAlumnos.data
  proyectos.value = resProyectos.data
  tiposParticipacion.value = resTipos.data
}

const obtenerAlumno = (id) => {
  const alumno = alumnos.value.find((a) => a.id === id)
  return alumno ? alumno.nombreCompleto : 'Sin alumno'
}

const obtenerProyecto = (id) => {
  const proyecto = proyectos.value.find((p) => p.id === id)
  return proyecto ? proyecto.nombre : 'Sin proyecto'
}

const obtenerTipo = (id) => {
  const tipo = tiposParticipacion.value.find((t) => t.id === id)
  return tipo ? tipo.nombre : 'Sin tipo de participación'
}

const eliminarParticipacion = async (id) => {
  const confirmar = confirm('¿Seguro que deseas eliminar esta participación?')

  if (confirmar) {
    await api.delete(`/participaciones/${id}`)
    cargarDatos()
  }
}

const seleccionarParticipacion = (participacion) => {
  emit('editar-participacion', participacion)
}

onMounted(() => {
  cargarDatos()
})
</script>

<template>
  <div>
    <h2>Lista de participaciones</h2>

    <p v-if="participaciones.length === 0">No hay participaciones registradas.</p>

    <table v-else border="1">
      <thead>
        <tr>
          <th>ID</th>
          <th>Alumno</th>
          <th>Proyecto</th>
          <th>Tipo de participación</th>
          <th>Fecha de inicio</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="participacion in participaciones" :key="participacion.id">
          <td>{{ participacion.id }}</td>
          <td>{{ obtenerAlumno(participacion.alumnoId) }}</td>
          <td>{{ obtenerProyecto(participacion.proyectoId) }}</td>
          <td>{{ obtenerTipo(participacion.tipoId) }}</td>
          <td>{{ participacion.fechaInicio }}</td>
          <td>
            <button @click="seleccionarParticipacion(participacion)">✏️</button>
            <button @click="eliminarParticipacion(participacion.id)">🗑️</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
