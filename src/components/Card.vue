<script setup lang="ts">
import { ref } from 'vue';

const {card} = defineProps(['card'])
const option = ref(card.option)

</script>

<template>
    <button :class="{selected: card.selected, solved: card.solved}" :disabled="card.solved"> 
        {{ option }}
        {{ card.row }}
        {{ card.col }}
    </button>
</template>

<style scoped>
button {
    border-radius: 10px;
    transition-property: grid-row, grid-col;
    transition-duration: 2s;
    margin: 1vw;
}

button.selected {
    background-color: burlywood;
}

button.solved {
    background-color: red;
    color: black;
}
</style>