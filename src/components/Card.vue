<script setup lang="ts">
import { ref } from 'vue';

const emit = defineEmits(['selectCard'])
const { card, disabled } = defineProps(['card', 'disabled'])
const cardText = ref(card.cardText)

// Try to distinguish non-mouse clicks (such as from screenreaders) with the detail property
// This is done to provide the touch experience where multiple cards can be tapped at once
function onClick(e: UIEvent) {
    if (e.detail === 0) emit('selectCard')
}
</script>

<template>
    <div class="wrapper">
        <button @touchstart.prevent="!(card.solved || disabled) && emit('selectCard')" @mousedown.left="emit('selectCard')" @click="onClick" :class="[
            card.selected ? 'selected' : '',
            card.solved ? `color${card.color + 1} solved` : ''
        ]" :disabled="card.solved || disabled">
            {{ cardText }}
        </button>
    </div>
</template>

<style scoped>
.wrapper {
    transition-property: grid-row, grid-col;
    transition-duration: 2s;
    display: grid;
    margin: 1vw;
}

button {
    border-radius: 10px;
    font-weight: 700;
    font-size: 20px;
    background-color: var(--card-button-bg);

    transition-property: background-color color;
    transition-duration: 0.2s !important;

    @media screen and (max-width: 720px) {
        font-size: 16px;
    }

    @media screen and (max-width: 480px) {
        font-size: 12px;
    }

    @media screen and (max-width: 320px) {
        font-size: 10px;
    }
}

button[disabled] {
    color: #c8c8c8;
    background-color: var(--card-button-bg-disabled);
}

button:hover:not([disabled]) {
    background-color: var(--card-button-bg-hover);
}

button:active:not([disabled]) {
    background-color: var(--card-button-bg-active);
}

button.selected {
    background-color: var(--card-button-bg-selected) !important;
}
</style>