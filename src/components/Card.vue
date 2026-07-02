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
    cursor: pointer;
    border-radius: 10px;
    font-weight: 700;
    font-size: large;
    border: none;

    transition-property: background-color;
    transition-duration: 0.1s;

    @media screen and (max-width: 720px) {
        font-size: medium;
    }

    @media screen and (max-width: 480px) {
        font-size: x-small;
    }
}

button[disabled] {
    cursor: auto;
}

button[disabled]:not(.solved) {
    background-color: #555555;
}

button.selected {
    background-color: burlywood;
}
</style>