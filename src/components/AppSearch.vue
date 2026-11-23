<script setup>
import { ref } from 'vue';

defineProps({
  titulo: String,
});

const emit = defineEmits(['buscar']);
const texto = ref('');

function buscar() {
  emit('buscar', texto.value);
}

function limpar() {
  texto.value = '';
  emit('buscar', '');
}
</script>

<template>
  <div class="container sticky-top mt-3 z-3">
    <div class="input-group shadow-sm">
      <span
        class="input-group-text bg-white border-end-0"
        style="cursor: pointer"
        @click="buscar"
      >
        <img class="iconesearch" src="/search.svg" width="20" alt="Buscar" />
      </span>

      <input
        v-model="texto"
        type="text"
        class="form-control border-start-0 border-end-0 ps-0"
        placeholder="Procure seu modelo 3D..."
        @keyup.enter="buscar"
      />

      <button
        class="btn btn-outline-secondary bg-white border-start-0 d-flex align-items-center"
        type="button"
        @click="limpar"
      >
        <img class="iconeclose" src="/close.svg" width="20" alt="Limpar" />
      </button>
    </div>
  </div>
</template>

<style scoped>
.z-3 {
  z-index: 1000;
}
.iconesearch,
.iconeclose {
  transition: all 0.3s ease;
}
.iconeclose:hover {
  filter: brightness(0) saturate(100%) invert(35%) sepia(85%) saturate(1200%)
    hue-rotate(340deg) brightness(95%) contrast(90%);
}
</style>
