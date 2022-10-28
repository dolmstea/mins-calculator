<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    modelValue: Boolean,
    title: String,
  },
  emits: ['update:modelValue'],
});
</script>

<template>
  <div @click="$emit('update:modelValue', !modelValue)" class="p-6 border-2 hover:bg-slate-200 border-slate-300 shadow rounded-lg text-5xl flex gap-4">
    <input class="min-w-[2rem] min-h-[2rem] my-auto" type="checkbox" v-model="modelValue" />
    <div class="flex flex-col shrink">
      <div class="text-2xl">{{ title }}</div>
      <div class="text-lg"><slot></slot></div>
    </div>
  </div>
</template>
