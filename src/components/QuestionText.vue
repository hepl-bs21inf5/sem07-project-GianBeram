<script setup lang="ts">
import { defineModel, defineProps } from 'vue'
const reponse_chat = defineModel<string | null>();
const props = defineProps({
  placeholder: {
    type: String,
    default: 'Entrez un nombre'
  }
})
</script>

<template>
<label for="exampleFormControlInput" class="form-label">
  Combien de pattes a un chat ?
</label>
<input
  id="exampleFormControlInput"
  v-model="reponse_chat"
  class="form-control"
  :placeholder="props.placeholder"
/>
</template>

<!--
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

