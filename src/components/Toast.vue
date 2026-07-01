<script setup lang = ts>
import { ref, useTemplateRef } from 'vue';

const visibleFor = 2000
const fadeoutLength = 3000

const fadeout = ref(false)

const message = ref('Hey')
const dialog = useTemplateRef<HTMLDialogElement>('dialog')

async function showMessage(newMessage: string) {
    message.value = newMessage
    dialog.value?.show()

    await new Promise(r => setTimeout(r, visibleFor))
    fadeout.value = true
    await new Promise(r => setTimeout(r, fadeoutLength))
    fadeout.value = false
    dialog.value?.close()
}

defineExpose({
    showMessage
})

</script>

<template>
    <dialog ref="dialog" :class="{fadeout}">
        {{ message }}
    </dialog>
</template>

<style scoped>
dialog.fadeout {
  animation: fadeout 3s ease-out forwards;
}

@keyframes fadeout{
  0%{
    opacity:1;
  }
  100%{
    opacity:0;
  }
}
</style>