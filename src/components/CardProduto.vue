<script setup>
const props = defineProps({
  titulo: String,
  valor: [String, Number],
  descricao: String,
  categoria: String,
  imagem: String,
});
</script>

<template>
  <div class="card h-100 shadow-sm custom-card">
    <img
      :src="imagem"
      class="card-img-top p-3 object-fit-contain"
      :alt="titulo"
      style="height: 200px"
    />

    <div class="card-body d-flex flex-column">
      <span class="badge text-bg-secondary mb-2 align-self-start">{{
        categoria
      }}</span>
      <h5 class="card-title fw-bold text-dark">{{ titulo }}</h5>
      <p class="card-text text-muted flex-grow-1">{{ descricao }}</p>
      <h5 class="card-subtitle mt-3 fw-bold text-primary">{{ valor }}</h5>
    </div>
  </div>
</template>

<style scoped>
.custom-card {
  transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
  cursor: pointer;
}

.custom-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
  border-color: #f00000 !important;
}
</style>
