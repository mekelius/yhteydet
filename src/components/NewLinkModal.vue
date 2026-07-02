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
        <div class="wrapper">

            <p class="instructions">
                Tässä linkki pulmaasi:
            </p>
        
            <a :href="link">{{ link }}</a> 
            
        <p>
            Pidä linkki tallessa. Jos hukkaat sen, joudut syöttämään pulmasi uudelleen.
        </p>
        <button @click="dialog!.close()">Sulje</button>
    </div>
    </dialog>
</template>

<style scoped>
dialog {
    max-width: 80vw;
    padding: 32px;
}

.wrapper {
    overflow-wrap: anywhere;
    display: flex;
    flex-direction: column;
    gap: 16px;
    align-items: center;
}

.instructions {
    font-size: 20px;
}

a {
    max-width: 100%;
    word-wrap: break-word;
}

button {
    font-weight: 500;
    font-size: 16px;
    padding: 16px 24px;
    margin-top: 16px;
    border-radius: 10px;
    border: none;
    max-width: max-content;
}
</style>