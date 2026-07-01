<script setup lang="ts">
import { ref, useTemplateRef } from 'vue';

const link = ref('')
const dialog = useTemplateRef('dialog')

function show(newLink: string) {
    link.value = newLink
    dialog.value?.showModal()
}

defineExpose({ show })

</script>

<template>
    <dialog ref="dialog">
        <span class="instructions">
            Voit jakaa tehtävän lähettämällä tämän linkin:
        </span>
        <br>
        <a :href="link">{{ link }}</a> <br>

        <button @click="dialog!.close()">Sulje</button>
    </dialog>
</template>

<style scoped>
dialog {
    max-width: 80vw;
    padding: 32px;
}

.instructions {
    font-size: large;
}

a {
    word-wrap: break-word;
}

button {
    font-weight: 500;
    font-size: medium;
    padding: 16px 24px;
    margin-top: 16px;
    border-radius: 10px;
    border: none;
}
</style>