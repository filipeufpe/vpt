<template>
  <div>
      <pre v-if="answer">{{ answer }}</pre>
  </div>
</template>

<script setup>
  defineProps({
      answer: {
          type: String,
          default: "A resposta aparecerá aqui."
      }
  })
</script>
