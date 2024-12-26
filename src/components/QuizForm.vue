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
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <form class="mb-4" @submit="submit">
          <QuestionCheckbox
            id="smash"
            v-model="questionStates[0]"
            text="Quel(s) personnage(s) n'est pas présent dans smash bros ultimate?"
            :answer="['waluigi']"
            answer-detail="Beaucoup de gens attendaient son arrivé sur smash ultimate."
            :options="[
              { value: 'pac-man', text: 'Pac-Man' },
              { value: 'waluigi', text: 'Waluigi' },
              { value: 'joker', text: 'Joker' },
              { value: 'sephiroth', text: 'Sephiroth' },
            ]"
          />
        </form>
        <form class="mb-4" @submit="submit">
          <QuestionText
            id="pokédex"
            v-model="questionStates[1]"
            text="Quel est le numéro de Arceus dans le pokédex"
            answer-detail="Victini est le 494ème, le premier pokémon du pokédex d'Unys."
            answer="493"
          />
        </form>
        <form class="mb-4" @submit="submit">
          <QuestionRadio
            id="lol"
            v-model="questionStates[2]"
            answer="kioz0m"
            text="Quel grand joueur suisse de league of legends à pris sa retraite récemment (le 8.12.2024) ?"
            answer-detail="Kioz0m le plus grand joueur de Suisse."
            :options="[
              { value: 'gobelet', text: 'Gobelet' },
              { value: 'kioz0m', text: 'Kioz0m' },
              { value: 'thyssouille', text: 'Thysouille' },
              { value: 'morinonovorra', text: 'Morinonovorra' },
            ]"
          />
        </form>
        <form class="mb-4" @submit="submit">
          <QuestionRadio
            id="pokemon"
            v-model="questionStates[3]"
            answer="chapignon"
            text="Quel est l'évolution de Balignon"
            answer-detail="C'est Chapignon le meilleur pokémon."
            :options="[
              { value: 'barguantua', text: 'Barguatua' },
              { value: 'cizayox', text: 'Cizayox' },
              { value: 'chapignon', text: 'Chapignon' },
              { value: 'clamiral', text: 'Clamiral' },
            ]"
          />
        </form>
        <form class="mb-4" @submit="submit">
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
          <form class="mb-4" @submit="submit">
            <QuestionCheckbox
              id="talent"
              v-model="questionStates[5]"
              text="Parmi ces pokémons, lesquel(s) possède(nt) le talent intimidation?"
              :answer="['demeteros-t', 'etouraptor']"
              answer-detail="Intimidation est un talent qui baisse l'attaque de un cran."
              :options="[
                { value: 'dialga', text: 'Dialga' },
                { value: 'demeteros-t', text: 'Démétéros-t' },
                { value: 'carchacrok', text: 'Carchacrok' },
                { value: 'etouraptor', text: 'Etouraptor' },
              ]"
            />
          </form>
          <div v-if="submitted">Score : {{ score }} / {{ totalScore }}</div>
          <button class="btn btn-primary" :class="{ disabled: !filled }" type="submit">
            Terminer
          </button>
        </form>
        <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
      </div>
    </div>
  </div>
</template>
