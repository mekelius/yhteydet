<script setup lang="ts">
const { category } = defineProps(['category'])
</script>

<template>
    <div>
        <h2>{{ category }}</h2>
    </div>
</template>

<style scoped>
div {
    position: relative;
    grid-column: 3;
    z-index: 2;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    padding-top: 16px;

    h2 {
        background-color: white;
        color: black;
        border: 1px solid black;
        position: absolute;
        border-radius: 10px;
        padding: 16px;
    }
}
</style>