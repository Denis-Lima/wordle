<script setup lang="ts">
import { WORD_SIZE } from '@/settings';
import englishWords from '@/englishWordsWith5Letters.json'
import { ref, watch } from 'vue'
import GuessView from './GuessView.vue';

const emit = defineEmits<{ "guess-submitted": [guess: string] }>()

const guessInProgress = ref<string>("")

function onSubmit() {
  if (!englishWords.includes(guessInProgress.value)) return

  emit("guess-submitted", guessInProgress.value)
  guessInProgress.value = ''
}

watch(guessInProgress, v => { guessInProgress.value = v.slice(0, WORD_SIZE).toUpperCase().replace(/[^A-Z]+/gi, '') })
</script>

<template>
  <GuessView :guess="guessInProgress" />
  <input type="text" v-model="guessInProgress" @keydown.enter="onSubmit" maxlength="WORD_SIZE" autofocus
    @blur="({ target }) => (target as HTMLInputElement).focus()">
</template>

<style scoped>
input {
  position: absolute;
  opacity: 0;
}
</style>