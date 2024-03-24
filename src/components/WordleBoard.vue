<script setup lang="ts">
import { DEFEAT_MESSAGE, VICTORY_MESSAGE, WORD_SIZE } from '@/settings';
import englishWords from '@/englishWordsWith5Letters.json'
import { ref, watch } from 'vue'

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) => englishWords.includes(wordGiven)
  }
})

const guessInProgress = ref<string>("")
const guessSubmitted = ref("")

function onSubmit() {
  if (!englishWords.includes(guessInProgress.value)) return
  guessSubmitted.value = guessInProgress.value
}

watch(guessInProgress, v => { guessInProgress.value = v.slice(0, WORD_SIZE).toUpperCase().replace(/[^A-Z]+/gi, '') })
</script>

<template>
  <input type="text" v-model="guessInProgress" @keydown.enter="onSubmit" maxlength="WORD_SIZE">
  <p v-if="guessSubmitted.length > 0" v-text="guessSubmitted === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE" />
</template>
