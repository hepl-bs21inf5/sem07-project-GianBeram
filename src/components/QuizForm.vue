<script setup lang="ts">
import QuestionRadio from '@/components/QuestionRadio.vue'
import QuestionText from '@/components/QuestionText.vue'
import { computed, ref } from 'vue'
import { QuestionState } from '@/utils/models'

const reponse_chat = ref<string | null>(null)

const questionStates = ref<QuestionState[]>([])

const score = computed<number>(
  () => questionStates.value.filter((state) => state === QuestionState.Correct).length,
)

const totalScore = computed<number>(() => questionStates.value.length)

const filled = computed<boolean>(() =>
  questionStates.value.every((state) => state === QuestionState.Fill),
)

const submitted = computed<boolean>(() =>
  questionStates.value.every(
    (state) => state === QuestionState.Correct || state === QuestionState.Wrong,
  ),
)

function reset(event: Event): void {
  event.preventDefault()
  questionStates.value = questionStates.value.map(() => QuestionState.Empty)
}

function submit(event: Event): void {
  event.preventDefault()
  questionStates.value = questionStates.value.map(() => QuestionState.Submit)
}
</script>

<template>
  <form @submit="submit">
    <QuestionText id="chat" v-model="reponse_chat" text="Combien de pattes a un chat ?" />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="cheval"
      v-model="questionStates[0]"
      answer="blanc"
      text="De quelle couleur est le cheval blanc de Napoléon ?"
      :options="[
        { value: 'blanc', text: 'Blanc' },
        { value: 'brun', text: 'Brun' },
        { value: 'noir', text: 'Noir' },
        { value: 'gris', text: 'Gris' },
      ]"
    />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="canton"
      v-model="questionStates[1]"
      answer="bern"
      text="Quelle est la capitale de la Suisse?"
      :options="[
        { value: 'bern', text: 'Bern' },
        { value: 'geneve', text: 'Genève' },
        { value: 'valais', text: 'Valais' },
        { value: 'bale', text: 'Bâle' },
      ]"
    />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="pattes"
      v-model="questionStates[2]"
      answer="quatre"
      text="Combien de pattes possède un chat?"
      :options="[
        { value: 'quatre', text: '4' },
        { value: 'une', text: '1' },
        { value: 'huit', text: '8' },
        { value: 'dix', text: '10' },
      ]"
    />
    <button class="btn btn-primary" :class="{ disabled: !filled }" type="submit">Terminer</button>
    <div>Debug états : {{ questionStates }}</div>
    <div v-if="submitted">Score : {{ score }} / {{ totalScore }}</div>
  </form>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template>
