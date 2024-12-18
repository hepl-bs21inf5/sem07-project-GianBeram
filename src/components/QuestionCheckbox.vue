<script setup lang="ts">
import { ref, watch, type PropType } from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>()
const props = defineProps({
  id: { type: String, required: true },
  text: { type: String, required: true },
  answer: { type: Array as PropType<Array<string>>, required: true },
  answerDetail: { type: String, default: '' },
  options: {
    type: Array as PropType<Array<{ value: string; text: string }>>,
    required: true,
  },
})

const checkedValues = ref<Array<string>>([])

watch(model, (newModel) => {
  if (newModel === QuestionState.Submit) {
    const isCorrect =
      checkedValues.value.length === props.answer.length &&
      props.answer.every((ans) => checkedValues.value.includes(ans))
    model.value = isCorrect ? QuestionState.Correct : QuestionState.Wrong //watch model avec chatgpt
  } else if (newModel === QuestionState.Empty) {
    checkedValues.value = []
  }
})

watch(
  checkedValues,
  (newValues) => {
    if (newValues.length === 0) {
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

<!--
<script setup lang="ts">
import { ref } from 'vue'
const checkedNames = ref([]);
</script>

<template>
<div class="form-check">
  <input
    id="checkboxJane"
    v-model="checkedNames"
    class="form-check-input"
    type="checkbox"
    value="Jane"
  />
  <label class="form-check-label" for="checkboxJane">Jane</label>
</div>
<div class="form-check">
  <input
    id="checkboxJohn"
    v-model="checkedNames"
    class="form-check-input"
    type="checkbox"
    value="John"
  />
  <label class="form-check-label" for="checkboxJohn">John</label>
</div>
</template>
-->
