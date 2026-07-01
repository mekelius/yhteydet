<script setup lang="ts">
import { ref } from 'vue';

const {card} = defineProps(['card'])
const option = ref(card.option)

</script>

<template>
    <button :class="{selected: card.selected}"> 
        {{ option }}
    </button>
</template>

<style scoped>
button.selected {
    background-color: burlywood;
}
</style>