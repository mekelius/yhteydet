<script setup lang="ts">
const { category, color } = defineProps(['category', 'color'])
</script>

<template>
    <div>
        <h2 :class="`header${color + 1}`">{{ category }}</h2>
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

    h2 {
        background-color: white;
        color: black;
        border: 1px solid black;
        position: absolute;
        top: 1.75vw;
        border-radius: 10px;
        word-wrap: normal;
        min-width: max-content;

        padding: 8px 16px;
        font-weight: 700;
        font-size: 16px;

        @media screen and (max-width: 480px) {
            font-size: 13px;
        }
    }
}
</style>