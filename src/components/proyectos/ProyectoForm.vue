<script setup>
import { ref, onMounted, watch } from 'vue'
import api from '../../services/api'

const props = defineProps({
  proyectoEditar: Object,
})

const emit = defineEmits(['proyecto-guardado'])

const docentes = ref([])

const proyecto = ref({
  nombre: '',
  descripcion: '',
  fechaInicio: '',
  docenteId: '',
})

const cargarDocentes = async () => {
  const response = await api.get('/docentes')
  docentes.value = response.data
}

const limpiarFormulario = () => {
  proyecto.value = {
    nombre: '',
    descripcion: '',
    fechaInicio: '',
    docenteId: '',
  }
}

const guardarProyecto = async () => {
  if (props.proyectoEditar) {
    await api.put(`/proyectos/${props.proyectoEditar.id}`, {
      ...proyecto.value,
      docenteId: Number(proyecto.value.docenteId),
    })
  } else {
    await api.post('/proyectos', {
      ...proyecto.value,
      docenteId: Number(proyecto.value.docenteId),
    })
  }

  limpiarFormulario()
  emit('proyecto-guardado')
}

watch(
  () => props.proyectoEditar,
  (nuevoProyecto) => {
    if (nuevoProyecto) {
      proyecto.value = { ...nuevoProyecto }
    }
  },
)

onMounted(() => {
  cargarDocentes()
})
</script>

<template>
  <form @submit.prevent="guardarProyecto">
    <h2>{{ proyectoEditar ? 'Editar proyecto' : 'Registrar proyecto' }}</h2>

    <div>
      <label>Nombre:</label>
      <input v-model="proyecto.nombre" type="text" required />
    </div>

    <div>
      <label>Descripción:</label>
      <textarea v-model="proyecto.descripcion" required></textarea>
    </div>

    <div>
      <label>Fecha de inicio:</label>
      <input v-model="proyecto.fechaInicio" type="date" required />
    </div>

    <div>
      <label>Docente:</label>
      <select v-model="proyecto.docenteId" required>
        <option value="">Seleccione un docente</option>
        <option v-for="docente in docentes" :key="docente.id" :value="docente.id">
          {{ docente.nombreCompleto }}
        </option>
      </select>
    </div>

    <button type="submit">
      {{ proyectoEditar ? 'Actualizar proyecto' : 'Guardar proyecto' }}
    </button>
  </form>
</template>
