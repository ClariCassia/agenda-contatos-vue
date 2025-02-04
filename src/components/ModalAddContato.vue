<template>

    <div v-if="mostrarModal" class="modal-overlay">

        <div class="modal contact-card">

            <h3>{{ editando ? 'Editar Contato' : 'Novo Contato' }}</h3>

            <form class="form" @submit.prevent="validarFormulario">
                <InputField v-model="contato.nome" type="text" placeholder="Nome" required />

                <InputField v-model="contato.email" type="email" placeholder="Email" />

                <InputField v-model="contato.telefone" placeholder="(11) 99999-9999" required
                    mask="'(##) #####-####'" />

                <InputField v-model="contato.imagem" type="text" placeholder="URL da Foto" />

                <div class="address-group">
                    <InputField v-model="contato.rua" type="text" placeholder="Rua" />
                    <InputField v-model="contato.numero" type="text" placeholder="Número" />
                    <InputField v-model="contato.bairro" type="text" placeholder="Bairro" />
                    <InputField v-model="contato.cidade" type="text" placeholder="Cidade" />
                </div>

                <div class="container-select">
                    <select v-model="contato.categoria" class="custom-select ">
                        <option value="" disabled class="placeholder">Selecione uma categoria:</option>
                        <option v-for="(label, index) in categorias" :key="index" :value="index">
                            {{ label }}
                        </option>
                    </select>

                    <select v-model="contato.uf" class="custom-select">
                        <option value="" disabled selected>Selecione o Estado:</option>
                        <option v-for="(sigla, index) in siglasUf" :key="index" :value="sigla">
                            {{ sigla }}
                        </option>
                    </select>
                </div>

                <div class="btn-group">
                    <button type="submit" class="btn ">
                        <i class="fas fa-floppy-disk"></i>
                    </button>
                    <button @click="$emit('fechar')" class="btn close">
                        <i class="fa-solid fa-x"></i>
                    </button>
                </div>

            </form>

        </div>
    </div>

</template>

<script>

import InputField from './InputField.vue';

export default {
    name: 'ModalAddContato',
    components: {
        InputField
    },
    props: {
        mostrarModal: Boolean,
        editando: Boolean,
        categorias: Object,
        valueContato: Object,

    },
    data() {
        return {
            siglasUf: [
                'AC', 'AL', 'AP', 'AM', 'BA', 'CE', 'DF', 'ES', 'GO', 'MA', 'MT', 'MS',
                'MG', 'PA', 'PB', 'PR', 'PE', 'PI', 'RJ', 'RN', 'RS', 'RO', 'RR', 'SC',
                'SP', 'SE', 'TO'
            ]
        };
    },

    computed: {
        contato: {
            get() {
                return this.valueContato;
            },
        }
    },

    methods: {
        async salvarContato(event) {

            try {
                let contatos = JSON.parse(localStorage.getItem('contatos')) || [];

                if (this.editando) {
                    const index = contatos.findIndex(contato => contato.id === this.contato.id);
                    if (index !== -1) {
                        contatos[index] = this.contato;
                    }
                } else {

                    this.contato.id = Date.now();
                    contatos.push(this.contato);
                }

                localStorage.setItem('contatos', JSON.stringify(contatos));

                this.$emit("fechar");
                this.$emit("atualizarLista");
            } catch (error) {
                console.error("Erro ao salvar contato", error);
            }
        },
        validarFormulario(event) {
            const form = event.target;
            if (form.checkValidity()) {
                this.salvarContato();
            } else {
                form.reportValidity();
            }
        },
    },
}
</script>

<style scoped>
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
}

.modal {
    background: #fff;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    text-align: center;
    width: 90%;
    max-width: 500px;
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.form {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

h3 {
    font-size: 1.5rem;

    margin-bottom: 10px;
}

.address-group {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
}

.container-select {
    display: flex;
    gap: 10px;
}

.btn-group {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
    margin-top: 10px;
}

.btn {
    background-color: transparent;
    border: none;
    cursor: pointer;
    padding: 8px;
    border-radius: 50%;
    font-size: 24px;
    color: var(--btnblue);
}

.btn:hover {

    color: var(--btnbluehover);
}

.btn i {
    font-size: var(--btn-font-size);
}

.btn.close {
    color: var(--btnred);
}

.btn.close:hover {
    color: var(--btnredhover);
}

.custom-select {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #fff;
    color: #333;
    font-size: 16px;
    outline: none;
    cursor: pointer;
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
    color: #333333b9;
}

.custom-select:focus {
    border-color: var(--focus-border-color);
    box-shadow: var(--focus-box-shadow);
    ;
}

.custom-select option {
    padding: 8px;
}

@media (max-width: 480px) {
    .modal {
        width: 95%;
    }

    .address-group {
        grid-template-columns: 1fr;
    }
}
</style>