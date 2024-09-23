<template>
  <n-form label-placement="left">
    <h1>Text to Speech</h1>
    <n-form-item>
      <n-input
        v-model:value="text"
        type="textarea"
        rows="6"
        placeholder="Enter text here..."
      />
    </n-form-item>

    <n-form-item>
      <n-button @click="speak" type="primary">Speak</n-button>
    </n-form-item>

    <!-- Voice Settings Component -->
    <voice-settings
      v-model:selectedVoice="voiceSettings.selectedVoice"
      v-model:rate="voiceSettings.rate"
      v-model:pitch="voiceSettings.pitch"
      :voice-options="voiceOptions"
    />

    <highlighted-text :text="text" :currentWordIndex="currentWordIndex" />
  </n-form>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { NForm, NFormItem, NButton, NInput } from "naive-ui";
import VoiceSettings from "./VoiceSettings.vue";
import HighlightedText from "./HighlightedText.vue";

const text = ref(`All that is gold does not glitter, 
Not all those who wander are lost; 
The old that is strong does not wither, 
Deep roots are not reached by the frost.`);

const voiceOptions = ref([]);
const voiceSettings = ref({
  selectedVoice: "",
  rate: 1,
  pitch: 1,
});


const populateVoiceList = () => {
  const voices = window.speechSynthesis.getVoices();
  if (voices.length > 0) {
    voiceOptions.value = voices.map((voice) => ({
      label: `${voice.name} (${voice.lang})`,
      value: voice.name,
    }));

    voiceSettings.value.selectedVoice = voices[0].name;
  }
};

const currentWordIndex = ref(-1);

const speak = () => {
  if (!text.value) {
    alert("Please enter some text.");
    return;
  }

  const utterance = new SpeechSynthesisUtterance(text.value);

  utterance.voice = window.speechSynthesis
    .getVoices()
    .find((voice) => voice.name === voiceSettings.value.selectedVoice);
  utterance.rate = voiceSettings.value.rate;
  utterance.pitch = voiceSettings.value.pitch;

  currentWordIndex.value = -1;

  utterance.onboundary = (event) => {
    if (event.name === "word") {
      const charIndex = event.charIndex;
      const wordsBeforeCurrent = text.value.slice(0, charIndex).split(" ");
      currentWordIndex.value = wordsBeforeCurrent.length - 1;
    }
  };

  window.speechSynthesis.speak(utterance);
};

onMounted(() => {
  populateVoiceList();
  if (window.speechSynthesis.onvoiceschanged !== undefined) {
    window.speechSynthesis.onvoiceschanged = populateVoiceList;
  }
});
</script>
