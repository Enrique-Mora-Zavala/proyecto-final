<script setup>
import { ref } from 'vue'
import AlumnoForm from '../components/alumnos/AlumnoForm.vue'
import AlumnosList from '../components/alumnos/AlumnosList.vue'

const actualizarLista = ref(0)
const alumnoSeleccionado = ref(null)

const recargarAlumnos = () => {
  actualizarLista.value++
  alumnoSeleccionado.value = null
}

const editarAlumno = (alumno) => {
  alumnoSeleccionado.value = alumno
}

const cancelarEdicion = () => {
  alumnoSeleccionado.value = null
}
</script>

<template>
  <div>
    <h1>Alumnos</h1>

    <AlumnoForm
      :alumnoEditar="alumnoSeleccionado"
      @alumno-guardado="recargarAlumnos"
      @cancelar-edicion="cancelarEdicion"
    />

    <hr />

    <AlumnosList :key="actualizarLista" @editar-alumno="editarAlumno" />
  </div>
</template>
