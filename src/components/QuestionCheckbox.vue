<script setup lang="ts">
import { ref, watch, type PropType, computed } from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>() // modèle lié à l'état de la question
const answerText = computed<string>(() => // associe chaque valeur à son texte associé, la méthode utilise map() et join() en plus
  props.answer.map((ans) => props.options.find((option) => option.value === ans)?.text ?? ans).join(', '),
) // map() parcout chaque élément de props.answer et associe le texte à la valeur, join () combine tous ce qui est obtenu par map() en une seule chaine de caractère qui utilise une virgule pour séparer les textes
const props = defineProps({
  id: { type: String, required: true }, // identifiant de la question
  text: { type: String, required: true }, // texte de la question
  answer: { type: Array as PropType<Array<string>>, required: true }, // réponse(s) de la question
  answerDetail: { type: String, default: 'Pas de détails supplémentaires disponibles.' }, // détail à propos de la question
  options: {
    type: Array as PropType<Array<{ value: string; text: string }>>, // liste des réponses possibles
    required: true,
  },
})

const checkedValues = ref<Array<string>>([]) // variable réactive qui contient le(s) case(s) cochée(s)

watch(model, (newModel) => {
  if (newModel === QuestionState.Submit) {
    const isCorrect =
      checkedValues.value.length === props.answer.length && // vérifie si le nombres cases cochées vaut le nombre de réponses correctes
      props.answer.every((ans) => checkedValues.value.includes(ans)) // vérifie si les réponses font parti des réponses cochées
    model.value = isCorrect ? QuestionState.Correct : QuestionState.Wrong
  } else if (newModel === QuestionState.Empty) {
    checkedValues.value = [] // si on ne répond pas à une question la liste est vide
  }
})

watch(
  checkedValues,
  (newValues) => {
    if (newValues.length === 0) {
      // ici on met un 0 car on est dans une liste, si la longueur de la liste vaut 0 donc une liste vide on est dans l'état empty
      model.value = QuestionState.Empty // si on ne répond pas à une question l'état est "vide"
    } else {
      model.value = QuestionState.Fill // si on répond à une question l'état est "remplis"
    }
  },
  { immediate: true }, // le watch est executé au début
)
</script>

<template>
  {{ props.text }}
  <div v-for="option in props.options" :key="option.value" class="form-check">
    <input
      :id="`${props.id}-${option.value}`"
      v-model="checkedValues"
      class="form-check-input"
      type="checkbox"
      :value="option.value"
      :disabled="
        model === QuestionState.Submit ||
        model === QuestionState.Correct ||
        model === QuestionState.Wrong
      "
    />
    <label class="form-check-label" :for="`${props.id}-${option.value}`">
      {{ option.text }}
    </label>
  </div>
  <div v-if="model === QuestionState.Correct || model === QuestionState.Wrong">
    <p v-if="model === QuestionState.Correct" class="text-success">Juste !</p>
    <p v-else class="text-danger">Faux ! La réponse était : {{ answerText }}</p>
    <p class="blockquote-footer">{{ props.answerDetail }}</p>
  </div>
</template>
