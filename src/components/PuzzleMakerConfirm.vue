<script setup lang="ts">
import { useTemplateRef } from 'vue';

const dialog = useTemplateRef('dialog')

function show() {
    dialog.value?.showModal()
}

function gotoPuzzleMaker() {
    const url = window.location.origin + window.location.pathname
    window.location.assign(url)
}

defineExpose({show})
</script>

<template>
    <dialog ref="dialog">
        Oletko varma, että haluat siirtyä luomaan uutta pulmaa?
        <div class="options">
            <button @click="gotoPuzzleMaker()">Kyllä (peliä ei tallenneta)</button>
            <button @click="dialog?.close()">Jatka peliä</button>
        </div>
    </dialog>
</template>

<style scoped>
.options {
    display: flex;
    gap: 16px;
    justify-content: center;

    @media screen and (max-width: 480px) {
        gap: 8px;
    }
}

button {
    font-weight: 500;
    font-size: medium;
    padding: 16px 24px;
    margin-top: 16px;

    @media screen and (max-width: 720px) {
        padding: 16px;
    }

    @media screen and (max-width: 480px) {
        gap: 8px;
        font-size: small;
        padding: 8px 16px;
    }
}
</style>