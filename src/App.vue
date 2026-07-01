<script setup lang="ts">
import { ref } from 'vue';

import Puzzle from './components/Puzzle.vue';
import PuzzleMaker from './components/PuzzleMaker.vue';

const currentView = ref<'puzzle' | 'puzzle-maker'>('puzzle')

const puzzle = [
    {
        category: 'Elukat',
        cards: ['Ankka', 'Lehmä', 'Valas', 'Jänes'],
    },
    {
        category: 'Juomat',
        cards: ['Vesi', 'Maito', 'Kalja', 'Kefiiri'],
    },
    {
        category: 'Ulokkeet',
        cards: ['Sarvi', 'Niemi', 'Jorma', 'Tatti'],
    },
    {
        category: 'Söpöt',
        cards: ['Miu', 'Mau', 'Jonna', 'Nasti'],
    },
]

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