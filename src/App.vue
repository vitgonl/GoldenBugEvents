<script setup>
import { ref, computed } from 'vue'
import Header from './components/Header.vue'
import SearchBar from './components/SearchBar.vue'
import EventCard from './components/EventCard.vue'
import Footer from './components/Footer.vue'

const meusEventos = ref([
  {
    id: 1,
    Nome: 'Festa de Halloween',
    Data: '30/10/2026',
    Local: 'Salão Golden Bug',
    Categoria: 'Festa',
    Descricao: 'Evento especial de Halloween',
  },
  {
    id: 2,
    Nome: 'Oficina Ubíqua',
    Data: '19/10/2026 a 24/10/2026',
    Local: 'Unama Alcino Cacela',
    Categoria: 'Acadêmico',
    Descricao: 'Evento acadêmico',
  },
  {
    id: 3,
    Nome: 'Show da Turnê Liniker Bye Bye Caju',
    Data: '07/11/2026',
    Local: 'Espaço Náutico Marine Club',
    Categoria: 'Música',
    Descricao: 'Evento musical',
  },
  {
    id: 4,
    Nome: 'Festival Psica 2026',
    Data: '10/12/2026',
    Local: 'Estádio Olímpico do Pará (Mangueirão)',
    Categoria: 'Música',
    Descricao: 'Festival de música',
  },
])

const pesquisa = ref('')
const categoriaSelecionada = ref('Todos')

const categorias = computed(() => [
  'Todos',
  ...new Set(meusEventos.value.map((evento) => evento.Categoria)),
])

function normalizar(texto) {
  return texto.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase()
}

const eventosFiltrados = computed(() => {
  return meusEventos.value.filter((evento) => {
    const combinaNome = normalizar(evento.Nome).includes(normalizar(pesquisa.value))
    const combinaCategoria =
      categoriaSelecionada.value === 'Todos' || evento.Categoria === categoriaSelecionada.value
    return combinaNome && combinaCategoria
  })
})

function mudarPesquisa(valor) {
  pesquisa.value = valor
}

function mudarCategoria(categoria) {
  categoriaSelecionada.value = categoria
}

// ---- favoritos ----
const favoritos = ref([])

function alternarFavorito(id) {
  if (favoritos.value.includes(id)) {
    favoritos.value = favoritos.value.filter((item) => item !== id)
  } else {
    favoritos.value.push(id)
  }
}

function estaFavorito(id) {
  return favoritos.value.includes(id)
}
</script>

<template>
  <Header />

  <main>
    <section id="inicio" class="inicio">
      <h2>Encontre seu próximo evento</h2>

      <p>
        Descubra eventos de música, esportes,
        tecnologia, cultura e muito mais.
      </p>
    </section>

    <section id="eventos" class="eventos">
      <h2>Eventos</h2>

      <SearchBar @pesquisar="mudarPesquisa" />

      <div class="categorias">
        <button
          v-for="categoria in categorias"
          :key="categoria"
          :class="{ ativo: categoriaSelecionada === categoria }"
          @click="mudarCategoria(categoria)"
        >
          {{ categoria }}
        </button>
      </div>

      <div class="contador">
        <span>{{ eventosFiltrados.length }} evento(s)</span>
        <span>{{ favoritos.length }} favorito(s)</span>
      </div>

      <div v-if="eventosFiltrados.length > 0" class="eventos-grid">
        <EventCard
          v-for="evento in eventosFiltrados"
          :key="evento.id"
          :Id="evento.id"
          :Nome="evento.Nome"
          :Data="evento.Data"
          :Local="evento.Local"
          :Categoria="evento.Categoria"
          :Descricao="evento.Descricao"
          :Favorito="estaFavorito(evento.id)"
          @favoritar="alternarFavorito"
        />
      </div>

      <p v-else class="sem-eventos">
        Nenhum evento encontrado. Tente outro nome ou escolha outra categoria.
      </p>
    </section>

    <section id="sobre" class="sobre">
      <h2>Sobre o GoldenBugEvents</h2>

      <p>
        O GoldenBugEvents é uma página para encontrar
        diferentes eventos.
      </p>
    </section>
  </main>

  <Footer />
</template>
