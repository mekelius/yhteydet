<script setup lang="ts">
import { ref } from 'vue';

const { card } = defineProps(['card'])
const cardText = ref(card.cardText)

</script>

<template>
    <div class="wrapper">
        <button :class="[card.selected ? 'selected' : '', card.solved ? `color${card.color + 1}` : '']" :disabled="card.solved">
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
    font-size: large;
    border: none;

    transition-property: background-color;
    transition-duration: 0.15s;
    
    @media screen and (max-width: 720px) {
        font-size: medium;
    }

    @media screen and (max-width: 480px) {
        font-size: x-small;
    }
}

button.selected {
    background-color: burlywood;
}
</style>