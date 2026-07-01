<script setup lang="ts">
import { computed, reactive } from 'vue';
import Card from './Card.vue';

const puzzle = [
    {
        category: 'category1',
        options: ['1a', '1b', '1c', '1d'],
    },
    {
        category: 'category2',
        options: ['2a', '2b', '2c', '2d'],
    },
    {
        category: 'category3',
        options: ['3a', '3b', '3c', '3d'],
    },
    {
        category: 'category4',
        options: ['4a', '4b', '4c', '4d'],
    },
]

const options = []
let id = 0;

for (const category of puzzle) {
    for (const option of category.options) {
        options.push({id, category: category.category, option, selected: false, solved: false})
        id++;
    }
}

const randomized = options.map((option) => ({pos: Math.random(), option}))
randomized.sort(({pos: pos1}, {pos:pos2}) => pos2 - pos1)

const cards = reactive(randomized.map(({option}) => option));

const numberOfSelected = computed(()=>cards.filter((v)=>v.selected).length);

function selectOption(id: number) {
    const card = cards.find((card) => card.id === id)!;
    if (card.selected)
        card.selected = false;

    if (numberOfSelected.value >= 4)
        return;

    card.selected = true
}

function declareWin() {
    window.alert("VOITIT PELIN");
}

function declareFail() {
    window.alert("VÄÄRIN MENI")
}

function makeGuess() {
    const selected = cards.filter(({selected}) => selected);

    if (selected.filter(({category}) => category === selected[0].category).length !== 4)
        return declareFail()    

    declareWin()
}

</script>

<template>
    kortit
    <ul>
        <Card v-for="card in cards" :card @click="() => selectOption(card.id)"/>
    </ul>

    <button @click="makeGuess" :disabled="numberOfSelected!=4">Arvaa</button>
</template>

<style scoped>
ul {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 20px;
    max-width: 200px;
}
</style>