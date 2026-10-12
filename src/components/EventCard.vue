<script setup>
import { ref } from 'vue'

defineProps({
  Id: Number,
  Nome: String,
  Data: String,
  Local: String,
  Categoria: String,
  Descricao: String,
  Favorito: Boolean,
})

const emit = defineEmits(['favoritar'])

const mostrarDetalhes = ref(false)
</script>

<template>
  <article class="card">
    <div class="card-topo">
      <span class="card-categoria">{{ Categoria }}</span>

      <button
        class="btn-favorito"
        :class="{ ativo: Favorito }"
        :aria-pressed="Favorito"
        :aria-label="Favorito ? 'Remover dos favoritos' : 'Adicionar aos favoritos'"
        @click="emit('favoritar', Id)"
      >
        {{ Favorito ? '♥' : '♡' }}
      </button>
    </div>

    <h3>{{ Nome }}</h3>

    <p class="card-info"><strong>Data</strong> {{ Data }}</p>
    <p class="card-info"><strong>Local</strong> {{ Local }}</p>

    <button class="btn-detalhes" @click="mostrarDetalhes = !mostrarDetalhes">
      {{ mostrarDetalhes ? 'Ocultar detalhes' : 'Ver detalhes' }}
    </button>

    <p v-show="mostrarDetalhes" class="card-detalhes">{{ Descricao }}</p>
  </article>
</template>
