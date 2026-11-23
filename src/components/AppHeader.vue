<script setup>
defineProps({
  titulo: String,
});
</script>

<template>
  <header
    class="navbar navbar-dark shadow-sm px-3 px-md-4"
    style="background-color: coral"
  >
    <div
      class="container-fluid d-flex justify-content-between align-items-center"
    >
      <img src="/MeshMakerLogo.svg" alt="MeshMaker Logo" style="width: 200px" />
      <span class="navbar-text text-white fw-semibold text-end">
        O lugar onde sua criatividade surge em formato 3D
      </span>
    </div>
  </header>
</template>

<style scoped></style>
