<template>
  <q-page>
    <div id="settings" style="margin: 0 20px 35px 0">
      <q-select
        v-model="category"
        :options="categories"
        option-value="id"
        option-label="name"
        label="Select Category"
      />
      <q-select
        v-model="difficulty"
        :options="['all', 'easy', 'medium', 'hard']"
        label="Select Difficulty"
      />
      <br />
      <q-btn
        id="fetchbtn"
        label="Fetch Questions"
        @click="fetchQuestions"
        style="margin-right: 10px"
      />
      <q-btn id="seebtn" label="See results" @click="seeResults" />
    </div>
    <q-table
      :key="key"
      :grid="isMobile"
      :data="questions"
      :columns="columns"
      row-key="question"
      :loading="loading"
      flat
      bordered
      no-data-label="No questions found"
      @row-click="openQuestionCard"
    />
    <q-dialog v-model="showQuestionCard" persistent>
      <question-card @answer="handleAnswer" :question="selectedQuestion" />
    </q-dialog>

    <q-dialog v-model="showResults" persistent>
      <score-card :answers="answers" />
    </q-dialog>
  </q-page>
</template>

<script>
import axios from "axios";
import QuestionCard from "./QuestionCard.vue";
import ScoreCard from "./ScoreCard.vue";

export default {
  components: { QuestionCard, ScoreCard },

  data() {
    return {
      key: 0,
      amount: 40,
      isMobile: false,
      loading: false,
      selectedQuestion: null,
      showQuestionCard: false,
      showResults: false,
      category: {
        id: 9,
        name: "General Knowledge",
      },
      difficulty: "all",
      categories: [],
      questions: [],
      answers: [],
      columns: [
        {
          format: (val, row) => {
            if (
              this.answers.find((answer) => answer.question === row.question)
            ) {
              return "\u2611";
            }
            return "\u2610";
          },
        },
        {
          name: "category",
          required: true,
          label: "Category",
          align: "left",
          field: "category",
        },
        {
          name: "difficulty",
          required: true,
          label: "Difficulty",
          align: "left",
          field: "difficulty",
        },
        {
          name: "question",
          required: true,
          label: "Question",
          align: "left",
          field: "question",
        },
      ],
    };
  },
  async created() {
    this.isMobile = this.$q.screen.lt.sm;
    this.categories = [
      { id: 9, name: "General Knowledge" },
      { id: 10, name: "Entertainment: Books" },
      { id: 11, name: "Entertainment: Film" },
      { id: 12, name: "Entertainment: Music" },
      { id: 13, name: "Entertainment: Musicals & Theatres" },
      { id: 14, name: "Entertainment: Television" },
      { id: 15, name: "Entertainment: Video Games" },
      { id: 16, name: "Entertainment: Board Games" },
      { id: 17, name: "Science & Nature" },
      { id: 18, name: "Science: Computers" },
      { id: 19, name: "Science: Mathematics" },
      { id: 20, name: "Mythology" },
      { id: 21, name: "Sports" },
      { id: 22, name: "Geography" },
      { id: 23, name: "History" },
      { id: 24, name: "Politics" },
      { id: 25, name: "Art" },
      { id: 26, name: "Celebrities" },
      { id: 27, name: "Animals" },
      { id: 28, name: "Vehicles" },
      { id: 29, name: "Entertainment: Comics" },
      { id: 30, name: "Science: Gadgets" },
      { id: 31, name: "Entertainment: Japanese Anime & Manga" },
      { id: 32, name: "Entertainment: Cartoon & Animations" },
    ];
    this.loadQuestionsFromLocalStorage();
    await this.fetchQuestions();
  },
  methods: {
    seeResults() {
      this.showResults = true;
    },
    loadQuestionsFromLocalStorage() {
      const keys = Object.keys(window.localStorage);
      const questionKeys = keys.filter((key) => key.includes("question-"));
      this.answers = questionKeys.map((key) =>
        JSON.parse(localStorage.getItem(key))
      );
    },
    handleAnswer(answer) {
      window.localStorage.setItem(
        `question-${answer.question}`,
        JSON.stringify(answer)
      );
      const previousAnswer = this.answers.find(
        (a) => a.question === answer.question
      );
      if (previousAnswer) {
        previousAnswer.answer = answer.answer;
      } else {
        this.answers.push(answer);
      }
      this.showQuestionCard = false;
      this.key++;
    },
    openQuestionCard(e, row) {
      this.selectedQuestion = row;
      this.showQuestionCard = true;
    },
    async fetchQuestions() {
      try {
        this.loading = true;
        const response = await axios.get(
          `https://opentdb.com/api.php?amount=${this.amount}&category=${
            this.category.id
          }&difficulty=${
            this.difficulty === "all" ? "" : this.difficulty
          }&encode=url3986`
        );
        this.questions = response.data.results
          .map((question) => {
            return {
              category: decodeURIComponent(question.category),
              difficulty: decodeURIComponent(question.difficulty),
              question: decodeURIComponent(question.question),
              correctAnswer: decodeURIComponent(question.correct_answer),
              incorrectAnswers: question.incorrect_answers.map((answer) =>
                decodeURIComponent(answer)
              ),
            };
          })
          .sort((a, b) => a.question.localeCompare(b.question));
      } catch (err) {
        console.error(err);
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style>
.q-page {
  max-width: 100vw;
  margin: auto;
  padding: 10px;
}
@media only screen and (max-width: 600px) {
  .q-btn{
    width: 35vw;
    min-width: 155px;
    margin-right: 2.5vw;
    margin-bottom: 2.5vw;
  }
}

@media only screen and (max-width: 390px) {
  .q-btn{
    width: 70vw;
    min-width: 50vw;
    margin-left: 8vw;
    margin-right: 8vw;
    margin-bottom: 2.5vw;
  }
}
 
@media only screen and (min-width: 1000px) {
  .q-page {
    max-width: 80vw;
  }
}
@media only screen and (min-width: 1400px) {
  .q-page {
    max-width: 60vw;
  }
}
</style>
