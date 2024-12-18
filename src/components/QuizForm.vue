<script setup lang="ts">
import QuestionRadio from '@/components/QuestionRadio.vue'
import QuestionText from '@/components/QuestionText.vue'
import QuestionCheckbox from '@/components/QuestionCheckbox.vue'
import { computed, ref } from 'vue'
import { QuestionState } from '@/utils/models'

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
    <QuestionCheckbox
      id="animaux"
      v-model="questionStates[0]"
      text="Quels sont les mammifères parmi les suivants?"
      :answer="['chien', 'chat']"
      answer-detail="Les serpents et les poissons font des oeufs."
      :options="[
        { value: 'chien', text: 'Chien' },
        { value: 'chat', text: 'Chat' },
        { value: 'serpent', text: 'Serpent' },
        { value: 'poisson', text: 'Poisson' },
      ]"
    />
  </form>
  <form @submit="submit">
    <QuestionText
      id="chat"
      v-model="questionStates[1]"
      text="Combien de pattes a un chat ?"
      answer-detail="Le chat est un mammifère quadrupède."
      answer="4"
    />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="lol"
      v-model="questionStates[2]"
      answer="kioz0m"
      text="Quel grand joueur suisse de league of legends à pris sa retraite récemment (le 8.12.2024) ?"
      answer-detail="Malheureusement ce n'est pas le très grand Gobelet."
      :options="[
        { value: 'gobelet', text: 'Gobelet' },
        { value: 'kioz0m', text: 'Kioz0m' },
        { value: 'thyssouille', text: 'Thysouille' },
        { value: 'morinonovorra', text: 'Morinonovorra' },
      ]"
    />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="pokemon"
      v-model="questionStates[3]"
      answer="chapignon"
      text="Quel est l'évolution de Balignon"
      answer-detail="C'est Chapignon le meilleure pokémon."
      :options="[
        { value: 'barguantua', text: 'Barguatua' },
        { value: 'cizayox', text: 'Cizayox' },
        { value: 'chapignon', text: 'Chapignon' },
        { value: 'clamiral', text: 'Clamiral' },
      ]"
    />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="mario"
      v-model="questionStates[4]"
      answer="ocicroc"
      text="Dans paper mario la porte millénaire quel est le boss final du donjon au 100 étages?"
      answer-detail="C'est ocicroc, les 3 sont des dragons et sont soeurs."
      :options="[
        { value: 'carbocroc', text: 'Carbocroc' },
        { value: 'toxicroc', text: 'Toxicroc' },
        { value: 'ocicroc', text: 'Ocicroc' },
      ]"
    />
    <div>Debug états : {{ questionStates }}</div>
    <div v-if="submitted">Score : {{ score }} / {{ totalScore }}</div>
    <button class="btn btn-primary" :class="{ disabled: !filled }" type="submit">Terminer</button>
  </form>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template>
