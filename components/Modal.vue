<template>
    <dialog class="modal" :class="theme">
        <h2>{{ message }}</h2>
        <div class="modal-btn-container">
            <form method="dialog">
                <button @click="quitGame">Quit</button>
                <button @click="restartGame">Try again</button>
            </form>
        </div>
    </dialog>
</template>


<script>
export default {
    props: ['theme', 'message'],
    emits: ['quit', 'restart'],
    methods: {
        quitGame() {
            this.$emit('quit');
        },
        restartGame() {
            this.$emit('restart');
        }
    }
}

</script>

<style scoped>
.modal {
    font-family: 'Changa One';
    border-radius: 20px;
    z-index: -1;
    width: 35rem;
    text-align: center;
}

.modal h2 {
    font-size: 5rem;
}

.modal::backdrop {
    background: hsl(0 100% 2% / 0.40)
}

@keyframes fadeIn {
    from {
        opacity: 0;
        margin-block-start: 0;
    }

    to {
        opacity: 1;
    }
}

dialog[open] {
    margin-block-start: 3rem;
    animation: fadeIn 0.3s ease-in normal;
}

.modal-btn-container {
    display: flex;
    margin-inline-end: auto;
}

.modal-btn-container {
    display: flex;
    justify-content: center;
    margin-inline-end: auto;
    margin-bottom: 20px;
}

.modal-btn-container form {
    width: 80%;
    display: flex;
    justify-content: space-between;
}

.modal-btn-container button {
    width: 160px;
    border: none;
    font-size: 2rem;
    border-radius: 5px;
    color: white;
    cursor: pointer;
    transition: all 250ms ease-in-out;
}

.modal-btn-container button:hover {
    transform: scale(1.1);
}

.modal-btn-container button:active {
    transform: scale(0.9);
}

.modal-btn-container button:first-of-type {
    background-color: green;
}

.modal-btn-container button:last-of-type {
    background-color: red;
}

.error {
    border: solid .5rem rgb(255, 166, 0);
}

.success {
    border: solid .5rem green;
}

@media only screen and (max-width: 768px) {
    .modal {
        border-radius: 20px;
        width: 25rem;
    }

    .modal h2 {
        font-size: 4rem;
    }

    .modal-btn-container button {
        width: 130px !important;
        font-size: 1.5rem;
    }
}

@media only screen and (max-width: 375px) {
    .modal {
        border-radius: 20px;
        width: 20rem;
        padding: 0px;
    }

    .modal h2 {
        font-size: 3rem;
    }

    .modal-btn-container button {
        width: 110px !important;
        font-size: 1.2rem;
    }
}
</style>