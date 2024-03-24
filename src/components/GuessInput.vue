<script setup lang="ts">
import { WORD_SIZE } from '@/settings';
import englishWords from '@/englishWordsWith5Letters.json'
import { ref, watch } from 'vue'
import GuessView from './GuessView.vue';

const emit = defineEmits<{ "guess-submitted": [guess: string] }>()

withDefaults(defineProps<{ disabled?: boolean }>(), { disabled: false })

const guessInProgress = ref<string>("")
const hasFailedValidation = ref(false)

function onSubmit() {
  if (!englishWords.includes(guessInProgress.value)) {
    hasFailedValidation.value = true
    setTimeout(() => {
      hasFailedValidation.value = false
    }, 500);

    return
  }

  emit("guess-submitted", guessInProgress.value)
  guessInProgress.value = ''
}

watch(guessInProgress, v => { guessInProgress.value = v.slice(0, WORD_SIZE).toUpperCase().replace(/[^A-Z]+/gi, '') })
</script>

<template>
  <GuessView :guess="guessInProgress" v-if="!disabled" :class="{ shake: hasFailedValidation }" />
  <input type="text" v-model="guessInProgress" @keydown.enter="onSubmit" maxlength="WORD_SIZE" autofocus
    @blur="({ target }) => (target as HTMLInputElement).focus()" :disabled="disabled">
</template>

<style scoped>
input {
  position: absolute;
  opacity: 0;
}

.shake {
  animation: shake;
  animation-duration: 100ms;
  animation-iteration-count: 2;
}

@keyframes shake {
  0% {
    transform: translate(-2%);
  }

  25% {
    transform: translate(0);
  }

  50% {
    transform: translate(2%);
  }

  75% {
    transform: translate(0);
  }
}
</style>