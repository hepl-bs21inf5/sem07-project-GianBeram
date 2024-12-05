<script setup lang="ts">
import { ref, watch } from 'vue'
import { QuestionState } from '@/utils/models'

const model = defineModel<QuestionState>()
const props = defineProps({
  id: { type: String, required: true },
  text: { type: String, required: true },
  answer: { type: String, required: true },
  placeholder: { type: String, default: 'Entrez un nombre'},
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
  <label for= "props.id" class= "form-label" >
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
</template>



<!--
<script setup lang="ts">
import { defineModel, defineProps } from 'vue'

const reponse_chat = defineModel<string | null>()
const props = defineProps({
  placeholder: {
    type: String,
    default: 'Entrez un nombre',
  },
})
</script>

<template>
  <label for="exampleFormControlInput" class="form-label"> Combien de pattes a un chat ? </label>
  <input
    id="exampleFormControlInput"
    v-model="reponse_chat"
    class="form-control"
    :placeholder="props.placeholder"
  />
</template>

<script setup lang="ts">
import { defineModel } from 'vue'
const reponse_chat = defineModel<string | null>();
</script>
<template>
<label for="exampleFormControlInput" class="form-label">
  Combien de pattes a un chat ?
</label>
<input
  id="exampleFormControlInput"
  v-model="reponse_chat"
  class="form-control"
  placeholder="Veuillez saisir un nombre"
/>
</template>
-->
