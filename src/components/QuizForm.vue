<script setup lang="ts">
import QuestionRadio from '@/components/QuestionRadio.vue'
import QuestionText from '@/components/QuestionText.vue'
import { computed, ref } from 'vue'

const cheval = ref<string | null>(null)

const canton = ref<string | null>(null)

const pattes = ref<string | null>(null)

const reponse_chat = ref<string | null>(null)

const correctAnswers = ref<boolean[]>([])

const score = computed<number>(() => correctAnswers.value.filter((value) => value).length);

const totalScore = computed<number>(() => correctAnswers.value.length);

const filled = computed<boolean>(
  () =>
    cheval.value !== null &&
    canton.value !== null &&
    pattes.value !== null &&
    reponse_chat.value !== null,
)

function reset() {
  cheval.value = null
  canton.value = null
  pattes.value = null
  reponse_chat.value = null
}

function submit(event: Event): void {
  let score = 0
  const max_score = 4
  let phrase = ''
  if (cheval.value == 'blanc') {
    score += 1
  }
  if (canton.value == 'bern') {
    score += 1
  }
  if (pattes.value == 'quatre') {
    score += 1
  }
  if (reponse_chat.value == 'quatre' || reponse_chat.value == 'Quatre' || reponse_chat.value == '4'){
    score += 1
  }
  if (score == max_score) {
    phrase = ' Vous avez un score parfait'
  } else if (score == 0) {
    phrase = ' la honte'
  } else if (score == max_score - 1) {
    phrase = ' tu y es presque'
  }
  event.preventDefault()
  if (filled.value) {
    alert(
      `Vous avez choisis ${cheval.value}, ${canton.value} et ${pattes.value} et votre score est de ${score}/${max_score}${phrase}  !`,
    )
  }
}
</script>

<template>
  <form @submit="submit">
    <QuestionText
      id="chat"
      v-model="reponse_chat"
      text="Combien de pattes a un chat ?"
    />
  </form>
  <form @submit="submit">
    <QuestionRadio
      id="cheval"
      v-model="correctAnswers[0]"
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
      v-model="correctAnswers[1]"
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
      v-model="correctAnswers[2]"
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
    <div>Réponses correctes : {{ correctAnswers }}</div>
    <div>Score : {{ score }} / {{ totalScore }}</div>
  </form>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template>

