<script setup lang="ts">
import { ref, watch } from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>()
const props = defineProps({
  id: { type: String, required: true },
  text: { type: String, required: true },
  answer: { type: String, required: true },
  answerDetail: { type: String, default: '' },
  placeholder: { type: String, default: 'Entrez un nombre' },
})

const value = ref<string | null>(null)

watch(model, (newModel) => {
  if (newModel === QuestionState.Submit) {
    model.value = value.value === props.answer ? QuestionState.Correct : QuestionState.Wrong
  } else if (newModel === QuestionState.Empty) {
    value.value = null
  }
})

watch(
  value,
  (newValue) => {
    if (newValue === null) {
      model.value = QuestionState.Empty
    } else {
      model.value = QuestionState.Fill
    }
  },
  { immediate: true },
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
