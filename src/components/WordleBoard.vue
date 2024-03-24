<script setup lang="ts">
import { DEFEAT_MESSAGE, VICTORY_MESSAGE, WORD_SIZE } from '@/settings';
import englishWords from '@/englishWordsWith5Letters.json'
import { ref, computed } from 'vue'

defineProps({
  wordOfTheDay: {
    type: String,
    validator: (wordGiven: string) => englishWords.includes(wordGiven)
  }
})

const guessInProgress = ref("")
const guessSubmitted = ref("")

const formattedGuessInProgress = computed({
  get() {
    return guessInProgress.value
  },
  set(rawValue: string) {
    guessInProgress.value = rawValue.slice(0, WORD_SIZE)
  }
})
</script>

<template>
  <input type="text" v-model="formattedGuessInProgress" @keydown.enter="guessSubmitted = guessInProgress"
    maxlength="WORD_SIZE">
  <p v-if="guessSubmitted.length > 0" v-text="guessSubmitted === wordOfTheDay ? VICTORY_MESSAGE : DEFEAT_MESSAGE" />
</template>
