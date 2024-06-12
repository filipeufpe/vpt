<template>
  <div>
    <div v-if="answer">
      <div v-html="marked.parse(answer)"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, defineProps } from "vue";
import { marked } from 'marked'

defineProps({
  answer: {
    type: String,
    default: "A resposta aparecerá aqui.",
  },
})
</script>
