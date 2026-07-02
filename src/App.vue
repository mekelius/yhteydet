<script setup lang="ts">
import { ref } from 'vue'

import Puzzle from './components/Puzzle.vue'
import PuzzleMaker from './components/PuzzleMaker.vue'

const ALLOWED_VERSION_STRINGS = ['puzzle', 'puzzlev2']

const queryParam = window.location.search.split('?')?.[1]

const versionString = queryParam?.split('=')?.[0]

if (queryParam && !versionString) {
    window.alert("Linkin tulkitseminen epäonnistui")
    throw "Missing version string"
}

if (queryParam && !(ALLOWED_VERSION_STRINGS.find(s => s === versionString))) {
    window.alert("Linkin tulkitseminen epäonnistui")
    throw `Unknown version string ${versionString}`
}

const puzzleParam = queryParam?.slice(versionString.length + 1)

function decodeAndValidateV1(puzzleParam: string) {
    try {
        const puzzle = JSON.parse(atob(puzzleParam))

        // validation here

        return puzzle
    } catch (e) {
        window.alert("Linkin tulkitseminen epäonnistui!")
        console.error(e)
    }
}

function decodeAndValidateV2(puzzleParam: string) {
    try {
        const puzzle = JSON.parse(atob(puzzleParam)).map(([title, cards]: [string, string[]]) => ({ title, cards }))

        // validation here

        return puzzle
    } catch (e) {
        window.alert("Linkin tulkitseminen epäonnistui!")
        console.error(e)
    }
}

const decodeAndValidate = versionString === 'puzzle' ? decodeAndValidateV1 : decodeAndValidateV2
const puzzle = puzzleParam ? decodeAndValidate(puzzleParam) : null
const currentView = ref<'puzzle' | 'puzzle-maker'>(puzzle ? 'puzzle' : 'puzzle-maker')

</script>

<template>
    <Puzzle v-if="currentView === 'puzzle'" :puzzle />
    <PuzzleMaker v-if="currentView === 'puzzle-maker'" />
</template>

<style>
@import "./colors.css";

/* 
    --small: 480px;
    --medium: 720px; 
*/

* {
    box-sizing: border-box;
}
</style>