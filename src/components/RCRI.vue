<script lang="ts">
import { defineComponent } from 'vue';
import type { PropType } from 'vue';
import RCRIQuestion from './RCRIQuestion.vue';

export default defineComponent({
  components: { RCRIQuestion },
  emits: ['answer'],
  methods: {
    handleSubmit() {
      let count = 0;

      for (let point of [this.highRisk, this.ihd, this.chf, this.cvd, this.insulin, this.creat]) {
        if (point) {
          count++;
        }
      }

      this.$emit('answer', count);
    },
  },
  data() {
    return {
      highRisk: false,
      ihd: false,
      chf: false,
      cvd: false,
      insulin: false,
      creat: false,
    };
  },
});
</script>

<template>
  <div class="container mx-auto px-4 py-6 flex flex-col items-stretch gap-8">
    <div class="font-sans text-7xl text-center">RCRI Score</div>
    <RCRIQuestion v-model="highRisk" title="Elevated-Risk Surgery"> Intraperitoneal, intrathoracic or suprainguinal vascular surgery. </RCRIQuestion>
    <RCRIQuestion v-model="ihd" title="Ischemic Heart Disease">
      History of myocardial infarction, history of positive exercise test, current chest pain considered to be due to myocardial ischemia, use of nitrate therapy, or ECG with pathological Q waves.
    </RCRIQuestion>
    <RCRIQuestion v-model="chf" title="Congestive Heart Failure">
      Pulmonary edema, bilateral rales or S3 gallop, paroxysmal nocturnal dyspnea, chest x-ray showing pulmonary vascular redistribution.
    </RCRIQuestion>
    <RCRIQuestion v-model="cvd" title="Cerebrovascular Disease"> Prior transient ischemic attack or stroke. </RCRIQuestion>
    <RCRIQuestion v-model="insulin" title="Insulin"> Pre-operative treatment with insulin. </RCRIQuestion>
    <RCRIQuestion v-model="creat" title="Pre-op Creatinine"> Pre-operative creatinine &gt; 176.8 mmol/L. </RCRIQuestion>
    <button @click="handleSubmit" class="p-6 border-2 hover:bg-slate-200 border-slate-300 shadow rounded-lg text-5xl">Next</button>
  </div>
</template>

<style></style>
