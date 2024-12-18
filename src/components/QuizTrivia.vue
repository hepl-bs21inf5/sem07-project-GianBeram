<script setup lang="ts">
import QuestionRadio from "@/components/QuestionRadio.vue";
import { reactive, ref, computed } from "vue";
import { QuestionState } from "@/utils/models";

const questions = ref<
  {
    question: string;
    correct_answer: string;
    incorrect_answers: string[];
  }[]
>([]);

const answers = reactive<{ [key: number]: QuestionState }>({});
const questionStates = ref<QuestionState[]>([]);

const score = computed<number>(
  () => questionStates.value.filter((state) => state === QuestionState.Correct).length
);

const totalScore = computed<number>(() => questionStates.value.length);

const filled = computed<boolean>(() =>
  questionStates.value.every((state) => state === QuestionState.Fill)
);

const submitted = computed<boolean>(() =>
  questionStates.value.every(
    (state) => state === QuestionState.Correct || state === QuestionState.Wrong
  )
);

function reset(event: Event): void {
  event.preventDefault();
  questionStates.value = questionStates.value.map(() => QuestionState.Empty);
}

function submit(event: Event): void {
  event.preventDefault();
  questionStates.value = questionStates.value.map(() => QuestionState.Submit);
}

fetch("https://opentdb.com/api.php?amount=10&type=multiple")
  .then((response) => response.json())
  .then((data) => {
    questions.value = data.results;
    questions.value.forEach((_, index) => {
      answers[index] = QuestionState.Empty; // Initialize question state
      questionStates.value.push(QuestionState.Empty);
    });
  });
</script>

<template>
  <form @submit="submit">
    <QuestionRadio
      v-for="(question, index) in questions"
      :id="index.toString()"
      :key="index"
      v-model="questionStates[index]"
      :text="question.question"
      :answer="question.correct_answer"
      :options="[
        { value: question.correct_answer, text: question.correct_answer },
        ...question.incorrect_answers.map(answer => ({
          value: answer,
          text: answer,
        }))
      ]"
    />
    <button class="btn btn-primary" :class="{ disabled: !filled }" type="submit">Terminer</button>
  </form>
  <div>Debug états : {{ questionStates }}</div>
  <div v-if="submitted">Score : {{ score }} / {{ totalScore }}</div>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template>
