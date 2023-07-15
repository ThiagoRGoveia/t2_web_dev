<template>
  <div>
    <q-card>
      <span class="row reverse">
        <q-btn icon="close" flat round dense v-close-popup />
      </span>
      <q-card-section>
        <div class="text-h6">{{ question.category }}</div>
        <div class="text-subtitle2">{{ question.question }}</div>
      </q-card-section>

      <q-separator />

      <q-card-section>
        <span class="column">
          <q-radio
            v-model="selectedAnswer"
            v-for="(answer, i) in getAllAnswers(question)"
            :key="i"
            :label="answer"
            :val="answer"
          />
        </span>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Save" @click="saveAnswer" />
      </q-card-actions>
    </q-card>
  </div>
</template>

<script>
export default {
  props: {
    question: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      selectedAnswer: null,
    };
  },
  methods: {
    getAllAnswers(question) {
      return [question.correctAnswer].concat(question.incorrectAnswers);
    },
    saveAnswer() {
      if (!this.selectedAnswer) {
        this.$q.notify({
          color: "red-5",
          textColor: "white",
          icon: "warning",
          message: "Please select an answer",
        });
      } else {
        this.$q.notify({
          color: "green-4",
          textColor: "white",
          icon: "cloud_done",
          message: "Your answer has been saved",
        });
        this.$emit("answer", {
          question: this.question.question,
          answer: this.selectedAnswer,
          correct: this.selectedAnswer === this.question.correctAnswer,
        });
      }
    },
  },
};
</script>
