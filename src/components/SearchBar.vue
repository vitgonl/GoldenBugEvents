<script setup>
import { ref } from 'vue'

const texto = ref('')
const emit = defineEmits(['pesquisar'])

function pesquisar() {
  emit('pesquisar', texto.value)
}
</script>

<template>
  <form class="search" @submit.prevent="pesquisar">
    <input
      v-model="texto"
      @input="pesquisar"
      type="text"
      placeholder="Pesquisar por nome do evento..."
    >
    <button type="submit">Pesquisar</button>
  </form>
</template>
