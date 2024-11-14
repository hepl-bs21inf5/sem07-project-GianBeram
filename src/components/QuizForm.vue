<script setup lang="ts">
import QuestionRadio from '@/components/QuestionRadio.vue'
import QuestionText from '@/components/QuestionText.vue'
import QuestionCheckbox from '@/components/QuestionCheckbox.vue'
import { computed, ref } from 'vue'

const cheval = ref<string | null>(null)

const canton = ref<string | null>(null)

const pattes = ref<string | null>(null)

const reponse_chat = ref<string | null>(null)

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
      v-model="cheval"
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
      v-model="canton"
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
      v-model="pattes"
      text="Combien de pattes possède un chat?"
      :options="[
        { value: 'quatre', text: '4' },
        { value: 'une', text: '1' },
        { value: 'huit', text: '8' },
        { value: 'dix', text: '10' },
      ]"
    />
    <button class="btn btn-primary" :class="{ disabled: !filled }" type="submit">Terminer</button>
  </form>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template>
<!-- <form @submit="submit">
    De quelle couleur est le cheval blanc de Napoléon ?
    <div class="form-check">
      <input
        id="chevalBlanc"
        v-model="cheval"
        class="form-check-input"
        type="radio"
        name="cheval"
        value="blanc"
      />
      <label class="form-check-label" for="chevalBlanc">Blanc</label>
    </div>
    <div class="form-check">
      <input
        id="chevalBrun"
        v-model="cheval"
        class="form-check-input"
        type="radio"
        name="cheval"
        value="brun"
      />
      <label class="form-check-label" for="chevalBrun">Brun</label>
    </div>
    <div class="form-check">
      <input
        id="chevalNoir"
        v-model="cheval"
        class="form-check-input"
        type="radio"
        name="cheval"
        value="noir"
      />
      <label class="form-check-label" for="chevalNoir">Noir</label>
    </div>
    <div class="form-check">
      <input
        id="chevalGris"
        v-model="cheval"
        class="form-check-input"
        type="radio"
        name="cheval"
        value="gris"
      />
      <label class="form-check-label" for="chevalGris">Gris</label>
    </div>
  </form>
  <form @submit="submit">
    Quel est la capitale de la Suisse
    <div class="form-check">
      <input
        id="Genève"
        v-model="canton"
        class="form-check-input"
        type="radio"
        name="canton"
        value="genève"
      />
      <label class="form-check-label" for="Genève">Genève</label>
    </div>
    <div class="form-check">
      <input
        id="Bern"
        v-model="canton"
        class="form-check-input"
        type="radio"
        name="canton"
        value="bern"
      />
      <label class="form-check-label" for="Bern">Bern</label>
    </div>
    <div class="form-check">
      <input
        id="Vaud"
        v-model="canton"
        class="form-check-input"
        type="radio"
        name="canton"
        value="vaud"
      />
      <label class="form-check-label" for="Vaud">Vaud</label>
    </div>
    <div class="form-check">
      <input
        id="Valais"
        v-model="canton"
        class="form-check-input"
        type="radio"
        name="canton"
        value="valais"
      />
      <label class="form-check-label" for="Valais">Valais</label>
    </div>
  </form>
  <form @submit="submit">
    Combien de pattes a un chat?
    <div class="form-check">
      <input
        id="Deux"
        v-model="pattes"
        class="form-check-input"
        type="radio"
        name="pattes"
        value="deux"
      />
      <label class="form-check-label" for="Deux">Deux</label>
    </div>
    <div class="form-check">
      <input
        id="Une"
        v-model="pattes"
        class="form-check-input"
        type="radio"
        name="pattes"
        value="une"
      />
      <label class="form-check-label" for="Une">Une</label>
    </div>
    <div class="form-check">
      <input
        id="Quatre"
        v-model="pattes"
        class="form-check-input"
        type="radio"
        name="pattes"
        value="quatre"
      />
      <label class="form-check-label" for="Quatre">Quatre</label>
    </div>
    <div class="form-check">
      <input
        id="Huit"
        v-model="pattes"
        class="form-check-input"
        type="radio"
        name="pattes"
        value="huit"
      />
      <label class="form-check-label" for="Huit">Huit</label>
    </div>
    <button class="btn btn-primary" :class="{ disabled: !filled }" type="submit">Terminer</button>
  </form>
  <button class="btn btn-secondary" @click="reset">Réinitialiser</button>
</template> -->
