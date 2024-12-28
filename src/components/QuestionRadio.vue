<script setup lang="ts">
import { ref, watch, type PropType, computed } from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>() // modèle lié à l'état de la question
const answerText = computed<string>( // computed qui contient le texte associé à une valeur
  () => props.options.find((option) => option.value === props.answer)?.text ?? props.answer,
)
const props = defineProps({
  id: { type: String, required: true }, // identifiant de la question
  text: { type: String, required: true }, // texte de la question
  answer: { type: String, required: true }, // réponse de la question
  answerDetail: { type: String, default: 'Pas de détails supplémentaires disponibles.' }, // détail à propos de la question
  options: {
    type: Array as PropType<Array<{ value: string; text: string }>>, // liste des réponses possibles
    required: true,
  },
})

const value = ref<string | null>(null) // variable réactive qui contient la réponse coché pour une question

watch(model, (newModel) => {
  if (newModel === QuestionState.Submit) {
    model.value = value.value === props.answer ? QuestionState.Correct : QuestionState.Wrong // si on répond à une question, vérifie si elle est juste ou fausse
  } else if (newModel === QuestionState.Empty) {
    value.value = null // si on ne répond pas à une question la valeur est vide
  }
})

watch(
  value,
  (newValue) => {
    if (newValue === null) {
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
      v-model="value"
      class="form-check-input"
      type="radio"
      :name="props.id"
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

