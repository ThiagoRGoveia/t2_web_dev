<template>
  <q-page>
    <q-table :data="results" :columns="columns" row-key="question" />
  </q-page>
</template>

<script>
export default {
  data() {
    return {
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
        {
          name: "correct_answer",
          required: true,
          label: "Correct Answer",
          align: "left",
          field: "correct_answer",
        },
        {
          name: "incorrect_answers",
          required: true,
          label: "Incorrect Answers",
          align: "left",
          field: (row) => row.incorrect_answers.join(", "),
        },
      ],
      results: [
        {
          category: "General Knowledge",
          type: "multiple",
          difficulty: "easy",
          question:
            "Which best selling toy of 1983 caused hysteria, resulting in riots breaking out in stores?",
          correct_answer: "Cabbage Patch Kids",
          incorrect_answers: [
            "Transformers",
            "Care Bears",
            "Rubik&rsquo;s Cube",
          ],
        },
        {
          category: "Geography",
          type: "multiple",
          difficulty: "medium",
          question:
            "The Principality of Sealand is an unrecognized micronation off the coast of what country?",
          correct_answer: "The United Kingdom",
          incorrect_answers: ["Japan", "Austrailia", "Argentina"],
        },
        {
          category: "Entertainment: Video Games",
          type: "multiple",
          difficulty: "medium",
          question:
            "In vanilla Minecraft, you can make armor out of all BUT which of the following?",
          correct_answer: "Emeralds",
          incorrect_answers: ["Diamonds", "Iron", "Leather"],
        },
        {
          category: "Entertainment: Music",
          type: "multiple",
          difficulty: "easy",
          question:
            "Sting, the lead vocalist of The Police, primarily plays what instrument?",
          correct_answer: "Bass Guitar",
          incorrect_answers: ["Drums", "Guitar", "Keyboards"],
        },
        {
          category: "Entertainment: Video Games",
          type: "multiple",
          difficulty: "hard",
          question:
            "What&#039;s the name of the halloween-related Sims 4 Stuff Pack released September 29th, 2015?",
          correct_answer: "Spooky Stuff",
          incorrect_answers: [
            "Ghosts n&#039; Ghouls",
            "Nerving Nights",
            "Fearful Frights",
          ],
        },
        {
          category: "Entertainment: Books",
          type: "multiple",
          difficulty: "medium",
          question: "The book &quot;The Little Prince&quot; was written by...",
          correct_answer: "Antoine de Saint-Exup&eacute;ry",
          incorrect_answers: [
            "Miguel de Cervantes Saavedra",
            "Jane Austen",
            "F. Scott Fitzgerald",
          ],
        },
        {
          category: "Entertainment: Music",
          type: "multiple",
          difficulty: "easy",
          question: "What album did Bon Iver release in 2016?",
          correct_answer: "22, A Million",
          incorrect_answers: [
            "Bon Iver, Bon Iver",
            "Blood Bank EP",
            "For Emma, Forever Ago",
          ],
        },
        {
          category: "Art",
          type: "boolean",
          difficulty: "easy",
          question: "Leonardo da Vinci was not the creator of the Mona Lisa.",
          correct_answer: "False",
          incorrect_answers: ["True"],
        },
        {
          category: "Entertainment: Music",
          type: "multiple",
          difficulty: "easy",
          question: "Which band recorded the album &quot;Parallel Lines&quot;?",
          correct_answer: "Blondie",
          incorrect_answers: ["Paramore", "Coldplay", "The Police"],
        },
        {
          category: "Sports",
          type: "multiple",
          difficulty: "easy",
          question: "Which player holds the NHL record of 2,857 points?",
          correct_answer: "Wayne Gretzky",
          incorrect_answers: ["Mario Lemieux ", "Sidney Crosby", "Gordie Howe"],
        },
      ],
    };
  },
  created() {
    // this.getData();
  },
  methods: {
    async getData() {
      // Call your API here and update the results data property
      // In your case, the data is hard-coded, so we can just assign it
      this.results = [];
    },
  },
};
</script>
