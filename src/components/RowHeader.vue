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
    background-color: blue;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    padding-top: 16px;

    h2 {
        position: absolute;
    }
}
</style>