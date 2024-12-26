<script setup lang="ts">
import { ref, watch, type PropType } from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>()
const props = defineProps({
  id: { type: String, required: true },
  text: { type: String, required: true },
  answer: { type: Array as PropType<Array<string>>, required: true },
  answerDetail: { type: String, default: 'Pas de détails supplémentaires disponibles.' },
  options: {
    type: Array as PropType<Array<{ value: string; text: string }>>,
    required: true,
  },
})

const checkedValues = ref<Array<string>>([]) //ref qui stock les cases cochées

watch(model, (newModel) => {
  if (newModel === QuestionState.Submit) {
    const isCorrect =
      checkedValues.value.length === props.answer.length && //vérifie si le nombres cases cochées vaut le nombre de réponses correctes
      props.answer.every((ans) => checkedValues.value.includes(ans)) //vérifie si les réponses font parti des réponses cochées
    model.value = isCorrect ? QuestionState.Correct : QuestionState.Wrong
  } else if (newModel === QuestionState.Empty) {
    checkedValues.value = []
  }
})

watch(
  checkedValues,
  (newValues) => {
    if (newValues.length === 0) { //ici on met un 0 car on est dans une liste, si la longueur de la liste vaut 0 donc une liste vide on est dans l'état empty
      model.value = QuestionState.Empty
    } else {
      model.value = QuestionState.Fill
    }
  },
  { immediate: true },
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
    <p v-else class="text-danger">Faux ! La réponse était : {{ answer }}</p>
    <p class="blockquote-footer">{{ props.answerDetail }}</p>
  </div>
</template>


