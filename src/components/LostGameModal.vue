<script setup lang="ts">
import { useTemplateRef } from 'vue';

const dialog = useTemplateRef('dialog')

function show() {
    dialog.value?.showModal()
}

defineExpose({ show })
</script>

<template>
    <dialog ref="dialog">
        Aijai, hävisit pelin
        <br>
        <button @click="dialog?.close()">Nyyh</button>
    </dialog>
</template>

<style scoped>
button {
    font-weight: 500;
    font-size: 16px;
    padding: 16px 24px;
    margin-top: 16px;
    max-width: max-content;
}
</style>