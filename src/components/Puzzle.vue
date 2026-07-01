<script setup lang="ts">
import { computed, onMounted, reactive, useTemplateRef } from 'vue';
import { wrapGrid } from 'animate-css-grid';
import Card from './Card.vue';
import Toast from './Toast.vue';
import RowHeader from './RowHeader.vue';

const puzzle = [
    {
        category: 'Elukat',
        options: ['Ankka', 'Lehmä', 'Valas', 'Jänes'],
    },
    {
        category: 'Juomat',
        options: ['Vesi', 'Maito', 'Kalja', 'Kefiiri'],
    },
    {
        category: 'Ulokkeet',
        options: ['Sarvi', 'Niemi', 'Jorma', 'Tatti'],
    },
    {
        category: 'Söpöt',
        options: ['Miu', 'Mau', 'Jonna', 'Nasti'],
    },
]

const options = []
let id = 0;

for (const category of puzzle) {
    for (const option of category.options) {
        options.push({ id, category: category.category, option, selected: false, solved: false })
        id++;
    }
}

const randomized = options.map((option) => ({ pos: Math.random(), option }))
randomized.sort(({ pos: pos1 }, { pos: pos2 }) => pos2 - pos1)

const cards = reactive(randomized.map(({ option }, index) => ({ ...option, row: Math.floor(index / 4) + 1, col: index % 4 + 1 })));

const numberOfSelected = computed(() => cards.filter((v) => v.selected).length);

function selectOption(id: number) {
    const card = cards.find((card) => card.id === id)!;
    if (card.selected)
        return card.selected = false;

    if (numberOfSelected.value >= 4)
        return;

    card.selected = true
}

const solvedRows = reactive<string[]>([])

function correctGuess(category: string) {
    solvedRows.push(category)

    const solved = cards.filter((card) => card.category === category)
    solved.forEach(card => { card.solved = true; card.selected = false })

    solved.forEach((card, index) => {
        if (card.row === solvedRows.length && card.col === index + 1)
            return

        const swappee = cards.find(({ row, col }) => row === solvedRows.length && col === index + 1)!
        swapCards(card.id, swappee.id)
    })
}

const toast = useTemplateRef('toast')

function declareFail() {
    toast.value!.showMessage('Väärin meni')
}

function makeGuess() {
    const selected = cards.filter(({ selected }) => selected);

    if (selected.filter(({ category }) => category === selected[0].category).length !== 4)
        return declareFail()

    correctGuess(selected[0].category)
}

function swapCards(card1ID: number, card2ID: number) {
    const card1 = cards.find((card) => card.id === card1ID)!
    const card2 = cards.find((card) => card.id === card2ID)!

    const row1 = card1.row
    const col1 = card1.col

    card1.row = card2.row
    card1.col = card2.col
    card2.row = row1
    card2.col = col1
}

const cardGrid = useTemplateRef('card-grid')

onMounted(() => {
    wrapGrid(cardGrid.value!, { easing: 'backOut', stagger: 10, duration: 400 })
})

</script>

<template>
    <div class="app">
        <div ref="card-grid" class="card-grid">
            <RowHeader v-for="category, row in solvedRows" :category :style="{ gridRow: row + 1 }" />

            <Card v-for="card in cards" :card @click="() => selectOption(card.id)"
                :style="{ gridRow: card.row, gridColumn: card.col + (card.col > 2 ? 1 : 0) }" />
        </div>
        <div class="controls">
            <button class="guess" @click="makeGuess" :disabled="numberOfSelected != 4">Arvaa</button>
        </div>
    </div>
    <Toast ref="toast" />
</template>

<style scoped>
.app {
    display: grid;
    grid-template-rows: 5fr 1fr;
    grid-template-columns: 100%;
    height: 100vh;
    width: 100%;
    padding-top: 5vh;
}

.card-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 0px 1fr 1fr;
    grid-template-rows: 1fr 1fr 1fr 1fr;
    /* gap: 20px; */
    width: 100%;
}

.controls {
    padding-left: 5%;
    padding-right: 5%;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;

    .guess {
        flex-grow: 2;
        padding: 20px;
        border-radius: 10px;
    }
}
</style>