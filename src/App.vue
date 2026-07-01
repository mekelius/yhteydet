<script setup lang="ts">
import { ref } from 'vue';

import Puzzle from './components/Puzzle.vue';
import PuzzleMaker from './components/PuzzleMaker.vue';

const puzzleParam = window.location.search.slice(8)

function decodeAndValidate(puzzleParam: string) {
    try {
        const puzzle = JSON.parse(atob(puzzleParam))

        // validation here
     
        console.log(puzzle)
        return puzzle
    } catch (e) {
        window.alert(e);
    }
}

console.log(puzzleParam)

const puzzle = puzzleParam ? decodeAndValidate(puzzleParam) : null
const currentView = ref<'puzzle' | 'puzzle-maker'>(puzzle ? 'puzzle' : 'puzzle-maker')

// const puzzle = [
//     {
//         title: 'Elukat',
//         cards: ['Ankka', 'Lehmä', 'Valas', 'Jänes'],
//     },
//     {
//         title: 'Juomat',
//         cards: ['Vesi', 'Maito', 'Kalja', 'Kefiiri'],
//     },
//     {
//         title: 'Ulokkeet',
//         cards: ['Sarvi', 'Niemi', 'Jorma', 'Tatti'],
//     },
//     {
//         title: 'Söpöt',
//         cards: ['Miu', 'Mau', 'Jonna', 'Nasti'],
//     },
// ]
</script>

<template>
  <Puzzle v-if="currentView === 'puzzle'" :puzzle />
  <PuzzleMaker v-if="currentView === 'puzzle-maker'" />
</template>

<style>
@import "./colors.css";

* {
    box-sizing: border-box;
}

</style>