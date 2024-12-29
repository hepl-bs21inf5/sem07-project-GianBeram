<script setup lang="ts">
import QuestionRadio from "@/components/QuestionRadio.vue";
import { reactive, ref, computed } from "vue";
import { QuestionState } from "@/utils/models";

const questions = ref< // contient les questions du quiz
  {
    question: string; // texte de la question
    correct_answer: string; // réponse correcte
    incorrect_answers: string[]; // listes des réponses fausses
  }[]
>([]);

const answers = reactive<{ [key: number]: QuestionState }>({}); // associe à chaque question son état

const questionStates = ref<QuestionState[]>([]); // variable réactive qui contient l'état des questions

const score = computed<number>(
  () => questionStates.value.filter((state) => state === QuestionState.Correct).length // computed qui contient le score du quiz
);

const totalScore = computed<number>(() => questionStates.value.length); // computed qui contient le score maximal du quiz

const filled = computed<boolean>(() =>
  questionStates.value.every((state) => state === QuestionState.Fill) // computed qui permet de terminer le quiz si l'état de toutes les questions est "filled"
);

const submitted = computed<boolean>(() =>
  questionStates.value.every(
    (state) => state === QuestionState.Correct || state === QuestionState.Wrong // computed qui indique si les questions sont justes ou fausses
  )
);

function reset(event: Event): void {
  event.preventDefault();
  questionStates.value = questionStates.value.map(() => QuestionState.Empty); // fonction qui reset le quiz
}

function submit(event: Event): void {
  event.preventDefault();
  questionStates.value = questionStates.value.map(() => QuestionState.Submit); // fonction qui termine le quiz
}

fetch("https://opentdb.com/api.php?amount=10&type=multiple")
  .then((response) => response.json()) 
  .then((data) => {
    questions.value = data.results; // met les questions dans la variable "questions"
    questions.value.forEach((_, index) => {
      answers[index] = QuestionState.Empty; // au début l'état de chaque question est empty
      questionStates.value.push(QuestionState.Empty); // ajoute les états à "questionStates"
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
  <div v-if="submitted">Score : {{ score }} / {{ totalScore }}</div>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template>
