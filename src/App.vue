<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokedex.jpg">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" placeholder="Buscar pokemon pelo nome" v-model="search" class="input is-rounded">
      <button class="button is-fullwidth is-success" id="searchBtn" @click="searchPoke">Buscar</button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <PokemonName :name="poke.name" :url="poke.url" :num="index + 1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import PokemonName from './components/PokemonName'

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: ''
    }
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results
      this.filteredPokemons = res.data.results
    })
  },
  components: {
    PokemonName
  },
  methods: {
    searchPoke: function() {
      this.filteredPokemons = this.pokemons
      if (this.search == '' || this.search == ' ') {
        this.filteredPokemons = this.pokemons
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.search)
      }
    }
  },
  computed: {
    /*resultSearch: function() {
      if (this.search == '' || this.search == ' ') {
        return this.pokemons
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.search)
      }
    }*/
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#searchBtn {
  margin-top: 2%
}
</style>
