<template>

    <div class="btn-container">
        <button class="btn" @click="() => abrirModal(false)" title="Adicionar Contato">
            Adicionar Contato <i class="fa-solid fa-plus"></i>
        </button>
    </div>

</template>

<script>

export default {
    name: "AddContactButton",
    props: {
        abrirModal: {
            type: Function,
            required: true,
        },
    },
};

</script>

<style scoped>
.btn-container {
    display: flex;
    justify-content: center;
    padding: 30px;
}

.btn {
    position: relative;
    transition: all 0.3s ease-in-out;
    box-shadow: 0px 5px 20px rgba(0, 0, 0, 0.2);
    padding-block: 0.5rem;
    padding-inline: 1.25rem;
    background-color: rgb(0 107 179);
    border-radius: 9999px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffff;
    gap: 10px;
    font-weight: bold;
    border: 3px solid #ffffff4d;
    outline: none;
    overflow: hidden;
    font-size: 15px;
    cursor: pointer;
}

.icon {
    width: 24px;
    height: 24px;
    transition: all 0.3s ease-in-out;
}

.btn:hover {
    transform: scale(1.05);
    border-color: #fff9;
}

.btn:hover .icon {
    transform: translate(4px);
}

.btn:hover::before {
    animation: shine 1.5s ease-out infinite;
}

.btn::before {
    content: "";
    position: absolute;
    width: 100px;
    height: 100%;
    background-image: linear-gradient(120deg, rgba(255, 255, 255, 0) 30%, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0) 70%);
    top: 0;
    left: -100px;
    opacity: 0.6;
}
</style>