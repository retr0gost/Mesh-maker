<script setup>
import { ref, computed } from 'vue';
import CardProduto from '../components/CardProduto.vue';
import AppSearch from '../components/AppSearch.vue';
import FooterCarrinho from '../components/FooterCarrinho.vue';
import PopupCarrinho from '../components/PopupCarrinho.vue';

const produtos = [
  {
    categoria: 'Fruta',
    imagem: '/bananafoda.jpeg',
    titulo: 'Chimpazine Bananine',
    descricao: 'Modelo 3D de Brainrot',
    valor: 'R$100,00',
  },
  {
    categoria: 'Modelo em 3D',
    imagem: '/hatsune.jpeg',
    titulo: 'Hatsune Miku',
    descricao: 'Cantora idol japonesa',
    valor: 'R$1.000,00',
  },
  {
    categoria: 'Modelos 3D',
    imagem: '/dragao3d.jpeg',
    titulo: 'Dragão articulado',
    descricao: 'Dragão impresso em uma peça só, super maneiro.',
    valor: 'R$89,90',
  },
  {
    categoria: 'Modelos 3D',
    imagem: '/minirobo.jpeg',
    titulo: 'Robozinho de mesa',
    descricao: 'Mini robô colecionável para decorar a sua a.',
    valor: 'R$39,90',
  },
  {
    categoria: 'Modelos 3D',
    imagem: '/plankton.jpeg',
    titulo: 'Plankton',
    descricao: 'Personagem vilão de bob esponja.',
    valor: 'R$45,90',
  },
  {
    categoria: 'Modelos 3D',
    imagem: '/tungtung.jpeg',
    titulo: 'Tung Tung Sahur',
    descricao: 'Personagem de I.A.',
    valor: 'R$49,90',
  },
  {
    categoria: 'Acessórios',
    imagem: '/suportecelular.jpeg',
    titulo: 'Suporte de celular',
    descricao: 'Suporte dobrável, leve e resistente.',
    valor: 'R$24,90',
  },
  {
    categoria: 'Acessórios',
    imagem: '/decoracaojojo.jpeg',
    titulo: 'Decoração',
    descricao:
      'Decorações para colocar em casa, fazemos outros modelos sob encomenda.',
    valor: 'R$35,90',
  },
  {
    categoria: 'Modelos 3D',
    imagem: '/ciro.jpeg',
    titulo: 'Ciro ai dentu',
    descricao: 'Ai dentu, compre logo sua replica do “ai dentu”.',
    valor: 'R$79,90',
  },
  {
    categoria: 'Filamentos',
    imagem: '/filamentopla.jpeg',
    titulo: 'Filamento PLA 1kg',
    descricao: 'Fácil de imprimir, ótimo para iniciantes.',
    valor: 'R$99,90',
  },
  {
    categoria: 'Filamentos',
    imagem: '/filamentopetg.jpeg',
    titulo: 'Filamento PETG 1kg',
    descricao: 'Mais resistente a impacto e calor que o PLA.',
    valor: 'R$119,90',
  },
  {
    categoria: 'Impressoras',
    imagem: '/impressora.jpeg',
    titulo: 'Impressora 3D FDM',
    descricao: 'Área de impressão 220x220x250mm, nivelamento automático.',
    valor: 'R$1.899,00',
  },
  {
    categoria: 'Impressoras',
    imagem: '/bicodeimpressora.jpeg',
    titulo: 'Bico 0.4mm (kit 5)',
    descricao: 'Kit de bicos de reposição em latão.',
    valor: 'R$19,90',
  },
];

const buscaAplicada = ref('');

function buscar(texto) {
  buscaAplicada.value = texto;
}

function normalizar(texto) {
  return texto
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .toLowerCase();
}

const produtosFiltrados = computed(() => {
  const termo = normalizar(buscaAplicada.value.trim());
  if (!termo) return produtos;
  return produtos.filter((p) =>
    normalizar(`${p.titulo} ${p.categoria} ${p.descricao}`).includes(termo)
  );
});

const carrinho = ref([]);
const popupAberto = ref(false);

function paraNumero(valor) {
  return Number(
    valor.replace('R$', '').trim().replace(/\./g, '').replace(',', '.')
  );
}

function paraMoeda(numero) {
  return numero.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
}

function adicionar(produto) {
  const existente = carrinho.value.find(
    (item) => item.titulo === produto.titulo
  );
  if (existente) {
    existente.quantidade++;
  } else {
    carrinho.value.push({
      titulo: produto.titulo,
      valor: produto.valor,
      preco: paraNumero(produto.valor),
      quantidade: 1,
    });
  }
}

function remover(titulo) {
  carrinho.value = carrinho.value.filter((item) => item.titulo !== titulo);
}

function comprar() {
  if (carrinho.value.length === 0) return;
  alert('Compra finalizada! Obrigado por comprar na MeshMaker 3D.');
  carrinho.value = [];
  popupAberto.value = false;
}

const totalItens = computed(() =>
  carrinho.value.reduce((soma, item) => soma + item.quantidade, 0)
);

const totalValor = computed(() =>
  carrinho.value.reduce((soma, item) => soma + item.preco * item.quantidade, 0)
);

const textoQuantidade = computed(
  () => `${totalItens.value} ${totalItens.value === 1 ? 'item' : 'itens'}`
);
</script>

<template>
  <AppSearch @buscar="buscar" />

  <main class="container my-4 pb-5 mb-5">
    <h1 class="text-center fw-bold mb-4">MeshMaker3D</h1>

    <div
      v-if="produtosFiltrados.length === 0"
      class="alert alert-warning text-center my-4"
      role="alert"
    >
      Nenhum produto encontrado.
    </div>

    <div class="row g-4">
      <div
        class="col-12 col-sm-6 col-md-4 col-lg-3"
        v-for="produto in produtosFiltrados"
        :key="produto.titulo"
      >
        <CardProduto
          :categoria="produto.categoria"
          :imagem="produto.imagem"
          :titulo="produto.titulo"
          :descricao="produto.descricao"
          :valor="produto.valor"
          @click="adicionar(produto)"
        />
      </div>
    </div>
  </main>

  <FooterCarrinho
    :valortotal="paraMoeda(totalValor)"
    :quantidadeitem="textoQuantidade"
    @abrir="popupAberto = true"
  />

  <PopupCarrinho
    v-if="popupAberto"
    :itens="carrinho"
    :total="paraMoeda(totalValor)"
    @fechar="popupAberto = false"
    @remover="remover"
    @comprar="comprar"
  />
</template>