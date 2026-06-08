<script setup>
import { ref } from 'vue'
import DocentesForm from '../components/docentes/DocentesForm.vue'
import DocentesList from '../components/docentes/DocentesList.vue'

const actualizarLista = ref(0)
const docenteSeleccionado = ref(null)

const recargarDocentes = () => {
  actualizarLista.value++
  docenteSeleccionado.value = null
}

const editarDocente = (docente) => {
  docenteSeleccionado.value = docente
}
</script>

<template>
  <div>
    <h1>Docentes</h1>

    <DocentesForm :docenteEditar="docenteSeleccionado" @docente-guardado="recargarDocentes" />

    <hr />

    <DocentesList :key="actualizarLista" @editar-docente="editarDocente" />
  </div>
</template>
