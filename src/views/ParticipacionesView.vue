<script setup>
import { ref } from 'vue'
import ParticipacionForm from '../components/participaciones/ParticipacionForm.vue'
import ParticipacionesList from '../components/participaciones/ParticipacionesList.vue'

const actualizarLista = ref(0)
const participacionSeleccionada = ref(null)

const recargarParticipaciones = () => {
  actualizarLista.value++
  participacionSeleccionada.value = null
}

const editarParticipacion = (participacion) => {
  participacionSeleccionada.value = participacion
}
</script>

<template>
  <div>
    <h1>Participaciones</h1>

    <ParticipacionForm
      :participacionEditar="participacionSeleccionada"
      @participacion-guardada="recargarParticipaciones"
    />

    <hr />

    <ParticipacionesList :key="actualizarLista" @editar-participacion="editarParticipacion" />
  </div>
</template>
