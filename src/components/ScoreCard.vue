<template>
  <div class="q-pa-md card-container">
    <span class="row reverse">
      <q-btn icon="close" flat round dense v-close-popup />
    </span>
    <div class="q-gutter-md row items-start justify-around">
      <div class="col">
        <q-card>
          <q-card-section class="bg-green text-white">
            <div class="text-h6">Score</div>
            <div class="text-h2">{{ correctAnswers }}</div>
          </q-card-section>
        </q-card>
      </div>
      <div class="col">
        <q-card>
          <q-card-section class="bg-red text-white">
            <div class="text-h6">Score</div>
            <div class="text-h2">{{ wrongAnswers }}</div>
          </q-card-section>
        </q-card>
      </div>
    </div>
    <br />

    <div class="q-gutter-md row">
      <div class="col">
        <q-card>
          <q-card-section>
            <div class="text-h6">Answers</div>
            <q-table
              :data="answers"
              :columns="columns"
              row-key="question"
            ></q-table>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    answers: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      columns: [
        {
          name: "question",
          required: true,
          label: "Question",
          align: "left",
          field: "question",
        },
        {
          name: "answer",
          required: true,
          label: "Answer",
          align: "left",
          field: "answer",
        },
        {
          name: "correct",
          required: true,
          label: "Correct",
          align: "left",
          field: "correct",
          format: (val) => (val ? "Yes" : "No"),
        },
      ],
    };
  },
  computed: {
    correctAnswers() {
      return this.answers.filter((answer) => answer.correct).length;
    },
    wrongAnswers() {
      return this.answers.filter((answer) => !answer.correct).length;
    },
  },
};
</script>

<style scoped>
.card-container {
  background-color: white;
  padding: 10px;
  max-width: none;
}
</style>
