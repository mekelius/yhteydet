<script setup lang="ts">
import { reactive } from 'vue';
import CategoryFieldSet from './CategoryFieldSet.vue';

const categories = reactive(Array(4).fill(null).map((_, index) => ({
    title: `Kategoria ${index + 1}`,
    color: String(index),
    cards: Array(4).fill('')
})))
</script>

<template>
    <div>
        <h1>Luo uusi pulma</h1>
        <form>
            <CategoryFieldSet v-model:title="categories[0].title" v-model:color="categories[0].color"
                v-model:card1="categories[0].cards[0]" v-model:card2="categories[0].cards[1]"
                v-model:card3="categories[0].cards[2]" v-model:card4="categories[0].cards[3]" />
            <CategoryFieldSet v-model:title="categories[1].title" v-model:color="categories[1].color"
                v-model:card1="categories[1].cards[0]" v-model:card2="categories[1].cards[1]"
                v-model:card3="categories[1].cards[2]" v-model:card4="categories[1].cards[3]" />
            <CategoryFieldSet v-model:title="categories[2].title" v-model:color="categories[2].color"
                v-model:card1="categories[2].cards[0]" v-model:card2="categories[2].cards[1]"
                v-model:card3="categories[2].cards[2]" v-model:card4="categories[2].cards[3]" />
            <CategoryFieldSet v-model:title="categories[3].title" v-model:color="categories[3].color"
                v-model:card1="categories[3].cards[0]" v-model:card2="categories[3].cards[1]"
                v-model:card3="categories[3].cards[2]" v-model:card4="categories[3].cards[3]" />
        </form>

        <button>Valmis</button>
    </div>
</template>

<style scoped>
    button {
        padding: 24px 32px;
        margin: 16px;
        font-size: large;
        font-weight: 700;
        border-radius: 10px;
        border: none;
    }
</style>