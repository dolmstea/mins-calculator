<script lang="ts">
import { defineComponent } from 'vue';
import Question from './components/Question.vue';
import RCRI from './components/RCRI.vue';

export default defineComponent({
  data() {
    return {
      currentQuestion: 'age',
      endCondition: '',
      age: '',
      cvd: '',
      urgency: '',
    };
  },
  components: {
    Question,
    RCRI,
  },
  methods: {
    answerAvailable() {
      return this.age === '<45' && this.cvd === 'no';
    },
    handleAge(age: string) {
      this.age = age;

      if (age === 'low' || age === 'med') {
        this.currentQuestion = 'cvd';
      } else {
        this.currentQuestion = 'urgency';
      }
    },
    handleCVD(cvd: string) {
      this.cvd = cvd;

      if (cvd == 'no') {
        this.endCondition = 'noMins';
      } else {
        this.currentQuestion = 'urgency';
      }
    },
    handleUrgency(urg: string) {
      switch (urg) {
        case 'emerg':
          if (this.age === 'high' || this.cvd === 'yes') {
            this.endCondition = 'minsEmerg';
          } else {
            this.endCondition = 'noMins';
          }
          break;
        case 'urg':
          if (this.age === 'high' || this.cvd === 'yes') {
            this.endCondition = 'minsUrg';
          } else {
            this.endCondition = 'noMins';
          }
          break;
        case 'elect':
          this.currentQuestion = 'rcri';
      }
    },
    handleRCRI(count: number) {
      if (this.age == 'high' || count >= 1 || (this.age == 'med' && this.cvd == 'yes')) {
        this.currentQuestion = 'bnp';
      } else {
        this.endCondition = 'mins';
      }
    },
    handleBNP(bnp: string) {
      if (bnp === 'pos' || bnp === 'na') {
        this.endCondition = 'mins';
      } else {
        this.endCondition = 'noMins';
      }
    },
  },
});
</script>

<template>
  <header></header>

  <main>
    <div v-if="endCondition === 'noMins'" class="container mx-auto px-4 py-6 flex flex-col items-stretch gap-8">
      <div class="font-sans text-7xl text-center">The patient does not require MINS surveillance.</div>
    </div>

    <div v-else-if="endCondition === 'minsEmerg'" class="container mx-auto px-4 py-6 flex flex-col items-stretch gap-8 font-sans">
      <div class="text-4xl text-center">Proceed to surgery without additional preoperative cardiac assessment.</div>
      <div class="text-2xl text-center">MINS surveillance indicated. Measure troponin daily x 48-72 hours. Obtain ECG in PACU. Consider in-hospital shared-care management.</div>
    </div>

    <div v-else-if="endCondition === 'minsUrg'" class="container mx-auto px-4 py-6 flex flex-col items-stretch gap-8 font-sans">
      <div class="text-4xl text-center">
        Proceed to surgery. Only undertake preoperative cardiac assessment if unstable cardiac condition or suspected undiagnosed severe pHTN or obstructive cardiac disease.
      </div>
      <div class="text-2xl text-center">MINS surveillance indicated. Measure troponin daily x 48-72 hours. Obtain ECG in PACU. Consider in-hospital shared-care management.</div>
    </div>

    <div v-else-if="endCondition === 'mins'" class="container mx-auto px-4 py-6 flex flex-col items-stretch gap-8 font-sans">
      <div class="text-4xl text-center">MINS surveillance indicated.</div>
      <div class="text-2xl text-center">Measure troponin daily x 48-72 hours. Obtain ECG in PACU. Consider in-hospital shared-care management.</div>
    </div>

    <template v-else>
      <Question v-if="currentQuestion === 'age'" q="Patient Age" :a="{ low: '18 - 44', med: '45 - 64', high: '&ge; 65' }" @answer="handleAge" />
      <Question v-if="currentQuestion === 'cvd'" q="Known Cardiovascular Disease?" :a="{ yes: 'Yes', no: 'No' }" @answer="handleCVD" />
      <Question v-if="currentQuestion === 'urgency'" q="Urgency of Surgery" :a="{ emerg: 'Emergent', urg: 'Urgent/Semiurgent', elect: 'Elective' }" @answer="handleUrgency" />
      <RCRI v-if="currentQuestion === 'rcri'" @answer="handleRCRI" />
      <Question v-if="currentQuestion === 'bnp'" q="Measure pre-op NT-proBNP or BNP." :a="{ pos: 'BNP &ge; 92 OR NT-proBNP &ge; 300', na: 'Not Available', neg: 'BNP Negative' }" @answer="handleBNP" />
    </template>
  </main>
</template>

<style></style>
