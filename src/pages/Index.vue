<template>
  <q-page>
    <questions-table />
  </q-page>
</template>

<script>
import QuestionsTable from "src/components/QuestionsTable.vue";
export default {
  components: { QuestionsTable },
  created() {
    console.log("created");
  },
};
</script>
