<script setup lang="ts">
import { reactive, useTemplateRef } from 'vue';
import CategoryFieldSet from './CategoryFieldSet.vue';
import NewLinkModal from './NewLinkModal.vue';

const newLinkModal = useTemplateRef('newLinkModal')

const categories = reactive(Array(4).fill(null).map((_, index) => ({
    title: `Kategoria ${index + 1}`,
    color: String(index),
    cards: Array(4).fill('')
})))

function swapColors(categoryID: number, newColor: string) {
    categories.find(({ color }) => color === newColor)!.color = categories[categoryID].color
    categories[categoryID].color = newColor
}

function makeLink() {
    const preparedCategories = [...categories]
        .sort(({color: lcolor}, {color: rcolor}) => Number(lcolor) - Number(rcolor))
        .map(({title, cards}) => ({title, cards}))

    const queryParam = btoa(JSON.stringify(preparedCategories))
    const link = `${window.location.href.split('?')[0]}?puzzle=${queryParam}`

    newLinkModal!.value!.show(link)
}
</script>

<template>
    <div>
        <h1>Luo uusi pulma</h1>
        <form @submit.prevent="makeLink()">
            <CategoryFieldSet v-model:title="categories[0].title" :color="categories[0].color"
                @update:color="(color) => swapColors(0, color)" v-model:card1="categories[0].cards[0]"
                v-model:card2="categories[0].cards[1]" v-model:card3="categories[0].cards[2]"
                v-model:card4="categories[0].cards[3]" />
            <CategoryFieldSet v-model:title="categories[1].title" :color="categories[1].color"
                @update:color="(color) => swapColors(1, color)" v-model:card1="categories[1].cards[0]"
                v-model:card2="categories[1].cards[1]" v-model:card3="categories[1].cards[2]"
                v-model:card4="categories[1].cards[3]" />
            <CategoryFieldSet v-model:title="categories[2].title" :color="categories[2].color"
                @update:color="(color) => swapColors(2, color)" v-model:card1="categories[2].cards[0]"
                v-model:card2="categories[2].cards[1]" v-model:card3="categories[2].cards[2]"
                v-model:card4="categories[2].cards[3]" />
            <CategoryFieldSet v-model:title="categories[3].title" :color="categories[3].color"
                @update:color="(color) => swapColors(3, color)" v-model:card1="categories[3].cards[0]"
                v-model:card2="categories[3].cards[1]" v-model:card3="categories[3].cards[2]"
                v-model:card4="categories[3].cards[3]" />

            <button>Valmis</button>
        </form>

        <NewLinkModal ref="newLinkModal"/>
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