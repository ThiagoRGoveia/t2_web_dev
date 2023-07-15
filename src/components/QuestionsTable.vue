<template>
  <q-page padding style="max-width: 60vw; margin: auto">
    <div style="margin: 0 20px 35px 0">
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
        label="Fetch Questions"
        @click="fetchQuestions"
        style="margin-right: 10px"
      />
      <q-btn label="See results" @click="seeResults" />
    </div>
    <q-table
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
  </q-page>
</template>

<script>
import axios from "axios";
import QuestionCard from "./QuestionCard.vue";

export default {
  components: { QuestionCard },

  data() {
    return {
      amount: 40,
      loading: false,
      selectedQuestion: null,
      showQuestionCard: false,
      category: {
        id: 9,
        name: "General Knowledge",
      },
      difficulty: "all",
      categories: [],
      questions: [],
      columns: [
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
    await this.fetchQuestions();
  },
  methods: {
    seeResults() {},
    handleAnswer(answer) {
      console.log(answer);
      this.showQuestionCard = false;
    },
    openQuestionCard(e, row) {
      console.log(row);
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
        this.questions = response.data.results.map((question) => {
          return {
            category: decodeURIComponent(question.category),
            difficulty: decodeURIComponent(question.difficulty),
            question: decodeURIComponent(question.question),
            correctAnswer: decodeURIComponent(question.correct_answer),
            incorrectAnswers: question.incorrect_answers.map((answer) =>
              decodeURIComponent(answer)
            ),
          };
        });
      } catch (err) {
        console.error(err);
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
