<template>
  <n-form label-placement="left">
    <n-form-item label="Voice">
      <n-select
        v-model:value="selectedVoice"
        :options="voiceOptions"
        placeholder="Select a voice"
      />
    </n-form-item>

    <n-form-item label="Rate">
      <n-slider v-model:value="rate" :min="0.5" :max="2" :step="0.1" />
    </n-form-item>

    <n-form-item label="Pitch">
      <n-slider v-model:value="pitch" :min="0" :max="2" :step="0.1" />
    </n-form-item>
  </n-form>
</template>

<script setup>
import { NForm, NFormItem, NSelect, NSlider } from "naive-ui";

const selectedVoice = defineModel("selectedVoice");
const rate = defineModel("rate");
const pitch = defineModel("pitch");

defineProps({
  voiceOptions: Array,
});
</script>
