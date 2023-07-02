<script>
import LivrosApi from "../api/livros";
import CategoriasApi from "../api/categorias";
const livrosApi = new LivrosApi();
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      livros: [],
      livro: {},
      categorias: [],
      categoria: {},
    };
  },

  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
  },


  methods: {
    async salvar() {
      if (this.livro.id) {
        await livrosApi.atualizarLivro(this.livro);
      } else {
        await livrosApi.adicionarLivro(this.livro);
      }
      this.livro = {};
      this.livro = await livrosApi.buscarTodosOsLivros();
    },
    editar(livro) {
      Object.assign(this.livro, livro);
    },
    async excluir(livro) {
      await livrosApi.excluirLivro(livro.id);
      this.livros = await livrosApi.buscarTodosOsLivros();
    },
  },
};
</script>
<template>
  <h1>Editoras</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="livro.titulo" placeholder="Título" />
    <input type="text" v-model="livro.isbn" placeholder="ISBN" />
    <input type="text" v-model="livro.quantidade" placeholder="Quantidade" />
    <input type="text" v-model="livro.preco" placeholder="Preço" />
    <!-- Ajustar pra aparecer categoria e autores -->
    <input type="text" v-model="livro.categoria" placeholder="Categoria" />
    <input type="text" v-model="livro.autores" placeholder="Autores" />
    <button @click="salvar">Salvar</button>
  </div>
  <hr />
  <ul>
    <li v-for="livro in livros" :key="livro.id">
      <span @click="editar(livro)">
        ({{ livro.id }}) - {{ livro.titulo }}
      </span>
      <button @click="excluir(livro)">X</button>
    </li>
  </ul>
</template>
