<script setup lang="ts">
import { ref } from 'vue';

const { card } = defineProps(['card'])
const cardText = ref(card.cardText)

</script>

<template>
    <button :class="[card.selected ? 'selected' : '', card.solved ? `color${card.color + 1}` : '']" :disabled="card.solved">
        {{ cardText }}
    </button>
</template>

<style scoped>
button {
    border-radius: 10px;
    transition-property: grid-row, grid-col;
    transition-duration: 2s;
    margin: 1vw;
    font-weight: 700;
    font-size: large;
    
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