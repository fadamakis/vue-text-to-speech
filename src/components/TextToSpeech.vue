<template>
  <h1>Text to Speech</h1>
  <n-input
    v-model:value="text"
    type="textarea"
    rows="6"
    placeholder="Enter text here..."
  />
  <n-button @click="speak" type="primary">Speak</n-button>
</template>

<script setup>
import { NButton, NInput } from "naive-ui";

import { ref } from "vue";

const text = ref(`
  All that is gold does not glitter,
  Not all those who wander are lost;
  The old that is strong does not wither,
  Deep roots are not reached by the frost.
`);

function speak() {
  if (text.value === "") {
    alert("Please enter some text.");
    return;
  }

  const utterance = new SpeechSynthesisUtterance(text.value);
  speechSynthesis.speak(utterance);
}
</script>
