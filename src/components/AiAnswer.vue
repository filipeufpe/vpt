<template>
  <div>
      <pre v-if="answer">{{ answer }}</pre>
  </div>
</template>

<script setup>
  defineProps({
      answer: {
          type: String,
          default: "Answer will be displayed here."
      }
  })
</script>
