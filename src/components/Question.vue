<script lang="ts">
import { defineComponent } from 'vue';
import type { PropType } from 'vue';

interface Answers {
  [answer: string]: string;
}

export default defineComponent({
  props: {
    q: String,
    a: Object as PropType<Answers>,
    modelValue: String,
  },
  emits: ['answer'],
});
</script>

<template>
  <div class="container mx-auto px-4 py-6 flex flex-col items-stretch gap-8">
    <div class="font-sans text-7xl text-center">{{ q }}</div>
    <button v-for="(value, key) in a" @click="$emit('answer', key)" class="p-6 border-2 hover:bg-slate-200 border-slate-300 shadow rounded-lg text-5xl">{{ value }}</button>
  </div>
</template>

<style></style>
