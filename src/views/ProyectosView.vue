<script setup>
import { ref } from 'vue'
import ProyectoForm from '../components/proyectos/ProyectoForm.vue'
import ProyectosList from '../components/proyectos/ProyectosList.vue'

const actualizarLista = ref(0)
const proyectoSeleccionado = ref(null)

const recargarProyectos = () => {
  actualizarLista.value++
  proyectoSeleccionado.value = null
}

const editarProyecto = (proyecto) => {
  proyectoSeleccionado.value = proyecto
}
</script>

<template>
  <div>
    <h1>Proyectos</h1>

    <ProyectoForm :proyectoEditar="proyectoSeleccionado" @proyecto-guardado="recargarProyectos" />

    <hr />

    <ProyectosList :key="actualizarLista" @editar-proyecto="editarProyecto" />
  </div>
</template>
