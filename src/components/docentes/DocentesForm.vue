<script setup>
import { ref, watch } from 'vue'
import api from '../../services/api'

const props = defineProps({
  docenteEditar: Object,
})

const emit = defineEmits(['docente-guardado', 'cancelar-edicion'])

const docente = ref({
  nombreCompleto: '',
})

const limpiarFormulario = () => {
  docente.value = {
    nombreCompleto: '',
  }
}

const guardarDocente = async () => {
  if (props.docenteEditar) {
    await api.put(`/docentes/${props.docenteEditar.id}`, {
      ...docente.value,
    })
  } else {
    await api.post('/docentes', {
      ...docente.value,
    })
  }

  limpiarFormulario()
  emit('docente-guardado')
}

watch(
  () => props.docenteEditar,
  (nuevoDocente) => {
    if (nuevoDocente) {
      docente.value = { ...nuevoDocente }
    }
  },
)
</script>

<template>
  <form @submit.prevent="guardarDocente">
    <h2>{{ docenteEditar ? 'Editar docente' : 'Registrar docente' }}</h2>

    <div>
      <label>Nombre completo:</label>
      <input v-model="docente.nombreCompleto" type="text" required />
    </div>

    <button type="submit">
      {{ docenteEditar ? 'Actualizar docente' : 'Guardar docente' }}
    </button>
  </form>
</template>
