<script setup>
import AppHeader from './components/AppHeader.vue';
</script>

<template>
  <AppHeader />

  <nav class="bg-light border-bottom py-2">
    <div class="container d-flex justify-content-center gap-3">
      <RouterLink to="/" class="btn btn-sm btn-outline-dark"> Loja </RouterLink>
      <RouterLink to="/sobre" class="btn btn-sm btn-outline-dark">
        Sobre a MeshMaker
      </RouterLink>
    </div>
  </nav>

  <RouterView />
</template>
