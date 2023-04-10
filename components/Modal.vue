<template>
    <dialog class="modal" :class="theme">
        <h2>{{ message }}</h2>
        <div class="modal-btn-container">
            <form method="dialog">
                <button>OK</button>
            </form>
        </div>
    </dialog>
</template>


<script>
    export default {
        props: ['theme', 'message'],
    }

</script>

<style scoped>
    .modal {
        font-family: 'Changa One';
        z-index: -1;
        width: 30rem;
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
        margin-block-start: 2rem;
        animation: fadeIn 0.3s ease-in normal;
    }

    .modal-btn-container {
        display: flex;
        margin-inline-end: auto;
    }

    .modal-btn-container button {
        border: none;
        background-color: transparent;
    }

    .error {
        border: solid .5rem red;
    }

    .success {
        border: solid .5rem green;
    }
</style>