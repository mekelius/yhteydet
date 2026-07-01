<script setup lang="ts">
import { ref } from 'vue'

import Puzzle from './components/Puzzle.vue'
import PuzzleMaker from './components/PuzzleMaker.vue'

const puzzleParam = window.location.search.slice(8)

function decodeAndValidate(puzzleParam: string) {
    try {
        const puzzle = JSON.parse(atob(puzzleParam))

        // validation here

        return puzzle
    } catch (e) {
        window.alert("Linkin tulkitseminen epäonnistui!")
        console.error(e)
    }
}

const puzzle = puzzleParam ? decodeAndValidate(puzzleParam) : null
const currentView = ref<'puzzle' | 'puzzle-maker'>(puzzle ? 'puzzle' : 'puzzle-maker')

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