<template>
  <div>
    <nav>
      <router-link to="/">Inicio</router-link> | <router-link to="/alumnos">Alumnos</router-link> |
      <router-link to="/proyectos">Proyectos</router-link> |
      <router-link to="/participaciones">Participaciones</router-link>
      <router-link to="/docentes">Docentes</router-link> |
    </nav>

    <hr />

    <router-view />
  </div>
</template>

<style>
nav {
  padding: 15px;
}

nav a {
  margin-right: 10px;
  text-decoration: none;
}
</style>
