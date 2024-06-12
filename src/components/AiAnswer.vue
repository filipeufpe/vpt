<template>
  <div>
      <pre v-if="answer">{{ answer }}</pre>
  </div>
</template>

<script setup lang="ts">
  defineProps({
      answer: {
          type: String,
          default: "A resposta aparecerá aqui."
      }
  })
</script>
