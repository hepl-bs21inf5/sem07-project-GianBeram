<script setup lang="ts">
import { ref, watch} from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>() // modèle lié à l'état de la question
const props = defineProps({
  id: { type: String, required: true }, // identifiant de la question
  text: { type: String, required: true }, // texte de la question
  answer: { type: String, required: true }, // réponse de la question
  answerDetail: { type: String, default: 'Pas de détails supplémentaires disponibles.' }, // détail à propos de la question
  placeholder: { type: String, default: 'Entrez un nombre' }, // texte affiché par défaut dans l'espace pour écrire
})

const value = ref<string | null>(null) // variable réactive qui contient la réponse écrite pour une question

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
  <label for="props.id" class="form-label">
    {{ props.text }}
  </label>
  <input
    :id="props.id"
    v-model="value"
    class="form-control"
    :disabled="
      model === QuestionState.Submit ||
      model === QuestionState.Correct ||
      model === QuestionState.Wrong
    "
    :placeholder="props.placeholder"
  />
  <div v-if="model === QuestionState.Correct || model === QuestionState.Wrong">
    <p v-if="model === QuestionState.Correct" class="text-success">Juste !</p>
    <p v-else class="text-danger">Faux ! La réponse était : {{ answer }}</p>
    <p class="blockquote-footer">{{ props.answerDetail }}</p>
  </div>
</template>

<style scoped>
.text-danger {
  color: red !important;
}
</style>
