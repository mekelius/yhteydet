<script setup lang="ts">
const {lives} = defineProps<{lives:number}>()
</script>

<template>
    <div class="wrapper">
        <div class="life" v-for="_, i in Array(4).fill(null)" :class="{gone: i >= lives}" ></div>
    </div>
</template>

<style scoped>
.wrapper {
    display: flex;
    justify-content: space-evenly;
    align-items: baseline;
}

.life {
    background-color: #777;
    width: 32px;
    height: 32px;
    border-radius: 100%;
}

.gone {
    visibility: hidden;
}
</style>