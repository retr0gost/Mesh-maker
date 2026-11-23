# MeshMaker 3D — Loja Virtual

Aplicação web front-end desenvolvida em **Vue 3** para exibição e venda de modelos 3D, filamentos (PLA e PETG), impressoras 3D e acessórios.

## Tecnologias Utilizadas

- **Vue 3** (`<script setup>`, Composition API com `ref`, `computed`, `defineProps` e `defineEmits`)
- **Vue Router** (navegação SPA entre as rotas `/` e `/sobre`)
- **Bootstrap 5** (sistema de grid responsivo, modal do carrinho, alertas e indicadores de carregamento)
- **Vite** (ferramenta de build e servidor de desenvolvimento)

## Estrutura do Projeto

- `src/components/`: Componentes reutilizáveis (`AppHeader.vue`, `AppSearch.vue`, `CardProduto.vue`, `FooterCarrinho.vue`, `PopupCarrinho.vue`)
- `src/views/`: Páginas da aplicação (`HomeView.vue` e `SobreView.vue`)
- `src/router.js`: Configuração das rotas do Vue Router

## API Integrada

- **Serviço:** ViaCEP (consulta automática de endereço por CEP para cálculo de entrega no carrinho)
- **Endpoint:** `https://viacep.com.br/ws/{cep}/json/`
- **Método HTTP:** `GET`
- **Formato dos dados:** `JSON`

## Como executar o projeto

```bash
# Instalar as dependências
npm install

# Iniciar o servidor de desenvolvimento
npm run dev
```
