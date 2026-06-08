<script setup>
import { ref } from 'vue'
import AlumnoForm from '../components/alumnos/AlumnoForm.vue'
import AlumnosList from '../components/alumnos/AlumnosList.vue'

const actualizarLista = ref(0)

const recargarAlumnos = () => {
  actualizarLista.value++
}
</script>

<template>
  <div>
    <h1>Alumnos</h1>

    <AlumnoForm @alumno-agregado="recargarAlumnos" />

    <hr />

    <AlumnosList :key="actualizarLista" />
  </div>
</template>
