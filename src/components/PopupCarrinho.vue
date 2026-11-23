<script setup>
import { ref } from 'vue';

defineProps({
  itens: Array,
  total: String,
});

defineEmits(['fechar', 'remover', 'comprar']);

// Estados reativos para a integração com a API ViaCEP
const cep = ref('');
const endereco = ref(null);
const carregandoCep = ref(false);
const erroCep = ref('');

async function buscarCep() {
  const cepLimpo = cep.value.replace(/\D/g, '');

  if (cepLimpo.length !== 8) {
    erroCep.value = 'Digite um CEP válido com 8 números.';
    endereco.value = null;
    return;
  }

  carregandoCep.value = true;
  erroCep.value = '';
  endereco.value = null;

  try {
    const resposta = await fetch(`https://viacep.com.br/ws/${cepLimpo}/json/`);

    if (!resposta.ok) {
      throw new Error('Falha na comunicação com o servidor.');
    }

    const dados = await resposta.json();

    if (dados.erro) {
      erroCep.value = 'CEP não encontrado.';
    } else {
      endereco.value = dados;
    }
  } catch (erro) {
    erroCep.value = 'Erro ao consultar o CEP. Tente novamente.';
  } finally {
    carregandoCep.value = false;
  }
}
</script>

<template>
  <div
    class="modal d-block custom-backdrop"
    tabindex="-1"
    @click.self="$emit('fechar')"
  >
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
      <div class="modal-content shadow-lg border-0 rounded-4">
        <div class="modal-header border-bottom-0 pb-0">
          <h5 class="modal-title fw-bold text-dark fs-4">Seu carrinho</h5>
          <button
            type="button"
            class="btn-close"
            aria-label="Fechar"
            @click="$emit('fechar')"
          ></button>
        </div>

        <div class="modal-body my-2">
          <div
            v-if="itens.length === 0"
            class="alert alert-secondary text-center my-3"
            role="alert"
          >
            Seu carrinho está vazio.
          </div>

          <div v-else>
            <ul class="list-group list-group-flush">
              <li
                v-for="item in itens"
                :key="item.titulo"
                class="list-group-item d-flex justify-content-between align-items-center px-0 py-3"
              >
                <div class="me-2">
                  <div class="fw-bold text-dark">{{ item.titulo }}</div>
                  <small class="text-muted"
                    >{{ item.quantidade }}x — {{ item.valor }}</small
                  >
                </div>

                <button
                  class="btn btn-sm btn-outline-danger border-0 rounded-circle px-2 py-1"
                  title="Remover item"
                  @click="$emit('remover', item.titulo)"
                >
                  <span class="fw-bold fs-6">✕</span>
                </button>
              </li>
            </ul>

            <!-- Consulta de CEP via API ViaCEP -->
            <div class="mt-3 pt-3 border-top">
              <label class="form-label fw-semibold text-dark small mb-1">
                Calcular entrega (CEP):
              </label>
              <div class="input-group input-group-sm">
                <input
                  v-model="cep"
                  type="text"
                  maxlength="9"
                  class="form-control"
                  placeholder="Ex: 66010-020"
                  @keyup.enter="buscarCep"
                />
                <button
                  type="button"
                  class="btn text-white fw-semibold custom-btn"
                  :disabled="carregandoCep"
                  @click="buscarCep"
                >
                  Buscar
                </button>
              </div>

              <!-- Estado de Carregamento -->
              <div v-if="carregandoCep" class="text-primary small mt-2">
                <span class="spinner-border spinner-border-sm me-1"></span>
                Consultando endereço...
              </div>

              <!-- Estado de Erro -->
              <div
                v-else-if="erroCep"
                class="alert alert-danger py-1 px-2 small mt-2 mb-0"
              >
                {{ erroCep }}
              </div>

              <!-- Estado de Sucesso (JSON renderizado) -->
              <div
                v-else-if="endereco"
                class="alert alert-success py-2 px-2 small mt-2 mb-0"
              >
                <div>
                  <strong>Entrega:</strong> {{ endereco.logradouro }},
                  {{ endereco.bairro }} — {{ endereco.localidade }}/{{
                    endereco.uf
                  }}
                </div>
                <div class="text-success fw-bold mt-1">
                  Frete estimado: Grátis
                </div>
              </div>
            </div>
          </div>
        </div>

        <div
          class="modal-footer border-top-0 flex-column align-items-stretch pt-0"
        >
          <div
            class="d-flex justify-content-between align-items-center mb-3 px-1"
          >
            <span class="fs-6 text-muted">Total:</span>
            <span class="fs-4 fw-bold text-primary">{{ total }}</span>
          </div>

          <button
            class="btn btn-primary py-2 fw-bold custom-btn"
            :disabled="itens.length === 0"
            @click="$emit('comprar')"
          >
            Comprar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.custom-backdrop {
  background-color: rgba(0, 0, 0, 0.6);
}

.custom-btn {
  background-color: #454ade;
  border-color: #454ade;
  transition: all 0.3s ease;
}

.custom-btn:hover:not(:disabled) {
  background-color: #252ad0;
  border-color: #252ad0;
  box-shadow: 0 4px 12px rgba(37, 42, 208, 0.3);
}
</style>
