<script setup lang="ts">
import { ref } from 'vue';

import Puzzle from './components/Puzzle.vue';
import PuzzleMaker from './components/PuzzleMaker.vue';

const currentView = ref<'puzzle' | 'puzzle-maker'>('puzzle')
</script>

<template>
  <Puzzle v-if="currentView === 'puzzle'" />
  <PuzzleMaker v-if="currentView === 'puzzle-maker'" />
</template>

<style>
* {
    box-sizing: border-box;
}
</style>