<script setup>
import { ref, onMounted, watch } from 'vue'
import api from '../../services/api'

const props = defineProps({
  alumnoEditar: Object,
})

const emit = defineEmits(['alumno-guardado', 'cancelar-edicion'])

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

const limpiarFormulario = () => {
  alumno.value = {
    nombreCompleto: '',
    numeroControl: '',
    carreraId: '',
    semestre: '',
  }
}

const cancelarEdicion = () => {
  limpiarFormulario()
  emit('cancelar-edicion')
}

const guardarAlumno = async () => {
  if (props.alumnoEditar) {
    await api.put(`/alumnos/${props.alumnoEditar.id}`, {
      ...alumno.value,
      carreraId: Number(alumno.value.carreraId),
      semestre: Number(alumno.value.semestre),
    })
  } else {
    await api.post('/alumnos', {
      ...alumno.value,
      carreraId: Number(alumno.value.carreraId),
      semestre: Number(alumno.value.semestre),
    })
  }

  limpiarFormulario()
  emit('alumno-guardado')
}

watch(
  () => props.alumnoEditar,
  (nuevoAlumno) => {
    if (nuevoAlumno) {
      alumno.value = { ...nuevoAlumno }
    }
  },
)

onMounted(() => {
  cargarCarreras()
})
</script>

<template>
  <form @submit.prevent="guardarAlumno">
    <h2>{{ alumnoEditar ? 'Editar alumno' : 'Registrar alumno' }}</h2>

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

    <button type="submit">
      {{ alumnoEditar ? 'Actualizar alumno' : 'Guardar alumno' }}
    </button>
    <button v-if="alumnoEditar" type="button" @click="cancelarEdicion">Cancelar</button>
  </form>
</template>
