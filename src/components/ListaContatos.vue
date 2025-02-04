<template>
    <div class="contact-grid">
        <CardContato v-for="(c, index) in contatos" :key="index" :contato="c" :index="index"
            @atualizar-lista="$emit('atualizar-lista')" :categorias="categorias" @editar="handleUpdate" />
    </div>
</template>

<script>
import CardContato from "./CardContato.vue";

export default {
    name: "ListaContatos",
    components: { CardContato },
    props: {
        contatos: Array,
        categorias: Object,
        contato: Object,
    },   
    methods: {
        handleUpdate(novoContato) {
            this.$emit('editar', novoContato);
        }
    },
};

</script>

<style scoped>
.contact-grid {
    display: flex;
    flex-direction: column;
    gap: 20px;
    justify-content: center;
    margin-top: 20px;
}
</style>