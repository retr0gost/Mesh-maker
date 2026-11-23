<script setup>
defineProps({
  valortotal: String,
  quantidadeitem: String,
});

const emit = defineEmits(['abrir']);

function abrirCarrinho() {
  emit('abrir');
}
</script>

<template>
  <div
    class="fixed-bottom bg-white border-top shadow-lg p-3 d-flex flex-column flex-sm-row justify-content-between align-items-center gap-3"
  >
    <div class="text-center text-sm-start flex-grow-1">
      <p class="mb-0 text-muted small">Total da compra</p>
      <span class="fs-5 fw-bold text-primary">{{ valortotal }}</span>
      <span class="text-dark"> / {{ quantidadeitem }}</span>
    </div>

    <div class="w-100 w-sm-auto">
      <button
        class="btn btn-primary w-100 d-flex align-items-center justify-content-center gap-2 px-4 py-2 custom-btn"
        @click="abrirCarrinho"
      >
        <span class="fw-bold">Ver carrinho</span>
        <img
          src="/triangle-up.svg"
          class="triangle-icon"
          width="15"
          height="15"
          alt="Seta para cima"
        />
      </button>
    </div>
  </div>
</template>

<style scoped>
.custom-btn {
  background-color: #454ade;
  border-color: #454ade;
  transition: all 0.3s ease;
}

.custom-btn:hover {
  background-color: #4a27c0;
  border-color: #4a27c0;
}

.triangle-icon {
  filter: brightness(0) invert(1);
}

@media (min-width: 576px) {
  .w-sm-auto {
    width: auto !important;
  }
}
</style>
