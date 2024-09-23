<template>
  <div class="highlighted-text">
    <span
      v-for="(word, index) in text.split(' ')"
      :key="index"
      :class="{ highlighted: index === currentWordIndex }"
      class="word"
    >
      {{ word }}
    </span>
  </div>
</template>

<script setup>
const props = defineProps({
  text: String,
  currentWordIndex: Number,
});
</script>

<style scoped>
.highlighted-text {
  margin-top: 20px;
  line-height: 1.5;
  white-space: pre;
}

.word::after {
  content: " ";
}

.highlighted {
  background-color: yellow;
  transition: background-color 0.3s ease;
}
</style>
