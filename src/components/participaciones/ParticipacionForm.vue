<script setup>
import { ref, onMounted, watch } from 'vue'
import api from '../../services/api'

const props = defineProps({
  participacionEditar: Object,
})

const emit = defineEmits(['participacion-guardada'])

const alumnos = ref([])
const proyectos = ref([])
const tiposParticipacion = ref([])

const participacion = ref({
  alumnoId: '',
  proyectoId: '',
  tipoParticipacionId: '',
  fechaInicio: '',
})

const cargarDatos = async () => {
  const resAlumnos = await api.get('/alumnos')
  const resProyectos = await api.get('/proyectos')
  const resTipos = await api.get('/tiposParticipacion')

  alumnos.value = resAlumnos.data
  proyectos.value = resProyectos.data
  tiposParticipacion.value = resTipos.data
}

const limpiarFormulario = () => {
  participacion.value = {
    alumnoId: '',
    proyectoId: '',
    tipoParticipacionId: '',
    fechaInicio: '',
  }
}

const guardarParticipacion = async () => {
  const datos = {
    ...participacion.value,
    alumnoId: Number(participacion.value.alumnoId),
    proyectoId: Number(participacion.value.proyectoId),
    tipoParticipacionId: Number(participacion.value.tipoParticipacionId),
  }

  if (props.participacionEditar) {
    await api.put(`/participaciones/${props.participacionEditar.id}`, datos)
  } else {
    await api.post('/participaciones', datos)
  }

  limpiarFormulario()
  emit('participacion-guardada')
}

watch(
  () => props.participacionEditar,
  (nuevaParticipacion) => {
    if (nuevaParticipacion) {
      participacion.value = { ...nuevaParticipacion }
    }
  },
)

onMounted(() => {
  cargarDatos()
})
</script>

<template>
  <form @submit.prevent="guardarParticipacion">
    <h2>{{ participacionEditar ? 'Editar participación' : 'Registrar participación' }}</h2>

    <div>
      <label>Alumno:</label>
      <select v-model="participacion.alumnoId" required>
        <option value="">Seleccione un alumno</option>
        <option v-for="alumno in alumnos" :key="alumno.id" :value="alumno.id">
          {{ alumno.id }} - {{ alumno.nombreCompleto }}
        </option>
      </select>
    </div>

    <div>
      <label>Proyecto:</label>
      <select v-model="participacion.proyectoId" required>
        <option value="">Seleccione un proyecto</option>
        <option v-for="proyecto in proyectos" :key="proyecto.id" :value="proyecto.id">
          {{ proyecto.id }} - {{ proyecto.nombre }}
        </option>
      </select>
    </div>

    <div>
      <label>Tipo de participación:</label>
      <select v-model="participacion.tipoParticipacionId" required>
        <option value="">Seleccione el tipo de participación</option>
        <option v-for="tipo in tiposParticipacion" :key="tipo.id" :value="tipo.id">
          {{ tipo.id }} - {{ tipo.nombre }}
        </option>
      </select>
    </div>

    <div>
      <label>Fecha de inicio:</label>
      <input v-model="participacion.fechaInicio" type="date" required />
    </div>

    <button type="submit">
      {{ participacionEditar ? 'Actualizar participación' : 'Guardar participación' }}
    </button>
  </form>
</template>
