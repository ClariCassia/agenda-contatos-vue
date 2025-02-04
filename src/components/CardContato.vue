<template>

  <div class="contact-card">

    <div class="contact-content">

      <img :src="contato.imagem || 'https://cdn-icons-png.flaticon.com/512/6073/6073873.png'" alt="Foto do Contato"
        class="contact-photo" />

      <div class="contact-details">
        <h3>{{ contato.nome }}</h3>
        <p><strong>Email:</strong> {{ contato.email }}</p>
        <p><strong>Telefone:</strong> {{ formatarTelefone(contato.telefone) }}</p>
        <p>
          <strong>Endereço:</strong>
          {{ contato.rua }}, {{ contato.numero }}, {{ contato.bairro }},
          {{ contato.cidade }}, {{ contato.uf }}
        </p>
      </div>

    </div>

    <div class="container">
      <div class="tag">
        <i class="fa-solid fa-star"></i>
        <span class="categoria">{{ contato.categoria }}</span>
      </div>

      <div class="btn-group">
        <button @click="$emit('editar', contato)" class="btn edit" title="Editar Contato">
          <i class="fa-solid fa-pen-to-square"></i>
        </button>

        <button @click="excluirContato(contato)" class="btn delete" title="Excluir Contato">
          <i class="fa-regular fa-trash-can"></i>
        </button>

      </div>

    </div>

  </div>
</template>

<script>
export default {
  name: "CardContato",
  props: {
    contato: Object,
    categorias: Object,
  },
  methods: {
    excluirContato(contato) {
      try {
        const contatosSalvos = JSON.parse(localStorage.getItem('contatos')) || [];

        const novosContatos = contatosSalvos.filter(item => item.id !== contato.id);

        localStorage.setItem('contatos', JSON.stringify(novosContatos));

        this.$emit("atualizar-lista");

        alert("Contato excluído com sucesso!");
      } catch (error) {
        console.error("Erro ao excluir contato", error);
      }
    },
    formatarTelefone(telefone) {
      if (!telefone) return '';
      const numero = telefone.replace(/\D/g, '');
      return numero.replace(/^(\d{2})(\d{5})(\d{4})$/, '($1) $2-$3');
    },
  },
};
</script>

<style scoped>
.contact-card {
  background: white;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

h3 {
  font-weight: bold;
}

strong {
  opacity: 0.7;
}

.contact-photo {
  width: 100px;
  height: 100px;
  border-radius: 50%;
}

.contact-details {
  text-align: justify;
  margin-left: 20px;
  padding-right: 20px;
}

.btn-group {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.btn {
  border: none;
  background: transparent;
  font-size: var(--btn-font-size);
  transition: var(--btn-transition);
  color: var(--btnblue);
}

.btn:hover {
  transform: scale(1.10);
}

.btn.edit:hover {
  color: var(--btnbluehover);
}

.btn.delete {
  color: var(--btnred);
}

.btn.delete:hover {
  color: var(--btnredhover);
}

.contact-content {
  display: flex;
  align-items: center;
}

.container {
  border-radius: 10px;
  padding: 10px 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  text-align: center;
}

.tag {
  display: flex;
  align-items: center;
  gap: 5px;
}

.tag i {
  display: flex;
  align-items: center;
  gap: 5px;
  color: #F9A825;
}

.tag i {
  font-size: 12px;
}

.categoria {
  font-size: 12px;
}

@media (max-width: 600px) {
  .contact-content {
    flex-direction: column;
    text-align: center;
  }

  .contact-details {
    text-align: center;
    margin-left: 0;
    padding: 0;
  }

  .btn-group {
    justify-content: center;
  }
}
</style>
