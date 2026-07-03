<script setup lang="ts">
import { computed, onMounted, reactive, ref, useTemplateRef } from 'vue';
import { wrapGrid } from 'animate-css-grid';
import Card from './Card.vue';
import Toast from './Toast.vue';
import RowHeader from './RowHeader.vue';
import PuzzleMakerConfirm from './PuzzleMakerConfirm.vue';
import Lives from './Lives.vue';
import LostGameModal from './LostGameModal.vue';

const toast = useTemplateRef('toast')
const cardGrid = useTemplateRef('card-grid')
const puzzleMakerConfirm = useTemplateRef('puzzle-maker-confirm')
const lostGameModal = useTemplateRef('lost-game-modal')

const lives = ref(4)

onMounted(() => {
    wrapGrid(cardGrid.value!, { easing: 'backInOut', stagger: 10, duration: 300 })
})

type Card = {
    id: number,
    cardText: string,
    category: string,
    selected: boolean,
    solved: boolean,
    row: number,
    col: number,
    color: number,
}

type Category = { title: string, cards: string[] }
type Puzzle = Category[]

const { puzzle } = defineProps<{ puzzle: Puzzle }>()

function initCards(puzzle: Puzzle): Card[] {
    const cards = puzzle.flatMap(({ title: category, cards }, color) => cards.map((cardText, i) => ({
        id: color * 4 + i,
        category,
        cardText,
        selected: false,
        solved: false,
        color,
    })))

    const randomized = cards.map((card) => ({ pos: Math.random(), card }))
    randomized.sort(({ pos: pos1 }, { pos: pos2 }) => pos2 - pos1)

    return randomized.map(({ card }, index) => ({
        ...card,
        row: Math.floor(index / 4) + 1,
        col: index % 4 + 1
    }))
}

const cards = reactive(initCards(puzzle));
const numberOfSelected = computed(() => cards.filter((v) => v.selected).length);

function selectOption(id: number) {
    const card = cards.find((card) => card.id === id)!;
    if (card.selected)
        return card.selected = false;

    if (numberOfSelected.value >= 4)
        return;

    card.selected = true
}

const solvedRows = reactive<{category: string, color: number}[]>([])

function correctGuess(category: string, color: number) {
    solvedRows.push({category, color})

    const solved = cards.filter((card) => card.category === category)
    solved.forEach(card => { card.solved = true; card.selected = false })

    solved.forEach((card, index) => {
        if (card.row === solvedRows.length && card.col === index + 1)
            return

        const swappee = cards.find(({ row, col }) => row === solvedRows.length && col === index + 1)!
        swapCards(card.id, swappee.id)
    })
}

function declareFail() {
    toast.value!.showMessage('Väärin meni')
    lives.value--
    if (lives.value <= 0)
        lostGameModal.value!.show()
}

function oneAway() {
    toast.value!.showMessage('Yksi on väärin')
    lives.value--
    if (lives.value <= 0)
        lostGameModal.value!.show()
}

function makeGuess() {
    const selected = cards.filter(({ selected }) => selected);

    const matchFirst = selected.filter(({ category }) => category === selected[0].category).length;
    if (matchFirst === 4)
        return correctGuess(selected[0].category, selected[0].color)

    if (matchFirst === 3)
        return oneAway()

    if (matchFirst === 1 && !selected.slice(2).find(({ category }) => category !== selected[1].category)) {
        return oneAway()
    }

    return declareFail()
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
</script>

<template>
    <div class="app">
        <button @click="puzzleMakerConfirm!.show()" class="puzzle-maker-button">Luo uusi pulma</button>
        <div ref="card-grid" class="card-grid">
            <RowHeader v-for="{category, color}, row in solvedRows" :category :color :style="{ gridRow: row + 1 }" />

            <Card v-for="card in cards" :card
                :style="{ gridRow: card.row, gridColumn: card.col + (card.col > 2 ? 1 : 0) }"
                :disabled="numberOfSelected >= 4 && !card.selected" @select-card="() => selectOption(card.id)" />
        </div>
        <div class="controls">
            <button class="guess" @click="makeGuess" :disabled="numberOfSelected != 4 || lives <= 0">Arvaa</button>
        </div>

        <Lives :lives />
    </div>

    <Toast ref="toast" />
    <PuzzleMakerConfirm ref="puzzle-maker-confirm" />
    <LostGameModal ref="lost-game-modal" />
</template>

<style scoped>
.puzzle-maker-button {
    position: fixed;
    top: 16px;
    right: 16px;
    padding: 8px 16px;
}

.app {
    display: grid;
    grid-template-rows: 5fr 1fr 0.6fr;
    grid-template-columns: 100%;
    height: 100dvh;
    width: 100%;
    padding-top: 64px;

    padding-left: 5vw;
    padding-right: 5vw;

    @media screen and (max-width: 480px) {
        padding-left: 1vw;
        padding-right: 1vw;
    }
}

.card-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 0px 1fr 1fr;
    grid-template-rows: 1fr 1fr 1fr 1fr;
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
        border: none;

        transition-duration: 0s !important;
    }

    .guess:hover:not([disabled]) {
        transition-duration: 0.1s !important;
    }

    .guess:active:not([disabled]) {
        transition-duration: 0.1s !important;
    }
}
</style>