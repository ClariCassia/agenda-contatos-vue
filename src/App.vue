<template>
  <div class="container">
    <h2>Agenda de Contatos</h2>

    <div class="group">
      <InputField type="text" v-model="filtro" placeholder="Buscar contato..." />
      <p v-if="!contatos || contatos.length === 0">Nenhum contato cadastrado</p>
    </div>

    <AddContactButton :abrirModal="abrirModal" />

    <ListaContatos :contatos="contatosFiltrados" :editando="editando" @atualizar-lista="carregarContatos"
      :categorias="categorias" @editar="editarContato" />

    <ModalAddContato :mostrarModal="mostrarModal" :editando="editando" :valueContato="contatoSelecionado"
      :categorias="categorias" @fechar="mostrarModal = false" @atualizar-lista="carregarContatos" />
  </div>
</template>

<script>
import ModalAddContato from "./components/ModalAddContato.vue";
import ListaContatos from "./components/ListaContatos.vue";
import AddContactButton from "./components/AddContactButton.vue";
import InputField from "./components/InputField.vue";

export default {
  components: {
    InputField,
    ModalAddContato,
    ListaContatos,
    AddContactButton,
  },
  data() {
    return {
      mostrarModal: false,
      editando: false,
      contatoSelecionado: null,
      contatos: [],
      filtro: "",
      categorias: {
        "Familia": "Família",
        "Amigos": "Amigos",
        "Trabalho": "Trabalho",
        "Outros": "Outros"
      },
    };
  },
  computed: {
    contatosFiltrados() {
      return this.contatos.filter(contato =>
        contato.nome.toLowerCase().includes(this.filtro.toLowerCase())
      );
    }
  },
  created() {
    this.carregarContatos();
  },
  methods: {
    carregarContatos() {
      const contatosSalvos = JSON.parse(localStorage.getItem('contatos')) || [];
      this.contatos = contatosSalvos;
    },
    abrirModal(editando, contato) {
      this.editando = editando;
      this.contatoSelecionado = contato || {};
      this.mostrarModal = true;
    },
    editarContato(contato) {
      this.abrirModal(true, contato);
    },
  }
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  text-align: center;
}

.group {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding-top: 50px;
}

h2 {
  font-family: 'Poppins', sans-serif;
  font-size: 2rem;
  color: #333;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.search-input {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
}
</style>
