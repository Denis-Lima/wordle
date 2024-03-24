<script setup lang="ts">
import { WORD_SIZE } from '@/settings';
import englishWords from '@/englishWordsWith5Letters.json'
import { ref, watch } from 'vue'

const emit = defineEmits<{ "guess-submitted": [guess: string] }>()

const guessInProgress = ref<string>("")

function onSubmit() {
  if (!englishWords.includes(guessInProgress.value)) return

  emit("guess-submitted", guessInProgress.value)
}

watch(guessInProgress, v => { guessInProgress.value = v.slice(0, WORD_SIZE).toUpperCase().replace(/[^A-Z]+/gi, '') })
</script>

<template>
  <input type="text" v-model="guessInProgress" @keydown.enter="onSubmit" maxlength="WORD_SIZE">
</template>
