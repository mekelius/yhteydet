<script setup lang="ts">
import type { Ref } from 'vue'

const title = defineModel('title')
const color = defineModel('color') as Ref<string>
const card1 = defineModel('card1')
const card2 = defineModel('card2')
const card3 = defineModel('card3')
const card4 = defineModel('card4')
</script>

<template>
    <fieldset>
        <div>
            <input required class="title" v-model="title" type="text">
            <select required v-model="color" :class="[`color${Number(color) + 1}`]">
                <option value="0" class="color1">Keltainen</option>
                <option value="1" class="color2">Vihreä</option>
                <option value="2" class="color3">Sininen</option>
                <option value="3" class="color4">Violetti</option>
            </select>
        </div>

        <div class="cards">
            <input required v-model="card1" type="text">
            <input required v-model="card2" type="text">
            <input required v-model="card3" type="text">
            <input required v-model="card4" type="text">
        </div>
    </fieldset>
</template>

<style scoped>
fieldset {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    padding: 24px;
    padding-bottom: 32px;
}

.title {
    font-weight: 700;
    font-size: medium;
    margin: 16px;
    margin-bottom: 32px;
}

.cards {
    display: flex;
    justify-content: space-around;

    @media screen and (max-width: 800px) {
        flex-direction: column;
        gap: 16px;
    }
}
</style>