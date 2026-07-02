<script setup lang="ts">
import { ref } from 'vue';

const emit = defineEmits(['selectCard'])
const { card, disabled } = defineProps(['card', 'disabled'])
const cardText = ref(card.cardText)

</script>

<template>
    <div class="wrapper">
        <button @click="emit('selectCard')"
            :class="[
                card.selected ? 'selected' : '', 
                card.solved ? `color${card.color + 1} solved` : ''
            ]"
            :disabled="card.solved || disabled">
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
    border: none;

    transition-property: background-color;
    transition-duration: 0.1s;

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

button[disabled]:not(.solved) {
    background-color: #606060;
    color: #c8c8c8;
}

button.selected {
    background-color: burlywood;
}
</style>