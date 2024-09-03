<template>
  <div class="app">
    <div class="columns">
      <input class="input" type="text" placeholder="Buscar Pokemon pelo nome" v-model="busca" />
      <button class="button is-small is-info" @click="buscar">Buscar</button>
    </div>

    <div class="columns is-multiline">

      <div class="column is-one-third" v-for="(poke, index) in filteredPoke" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>

    </div>
  </div>
</template>

<script>

import axios from "axios"
import Pokemon from "./components/Pokemon.vue";

const url = "https://pokeapi.co/api/v2/pokemon?limit=151&offset=0"

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPoke: [],
      busca: ''
    }
  },
  created: function () {
    axios.get(url).then(res => {
      this.pokemons = res.data.results
      this.filteredPoke = res.data.results
    })
  }, components: {
    Pokemon
  },
  methods: {
    buscar: function () {
      this.filteredPoke = this.pokemons
      if (this.busca == '' || this.busca == ' ') {
        this.filteredPoke = this.pokemons
      } else {
        this.filteredPoke = this.pokemons.filter(pokemon => pokemon.name.includes(this.busca.trim().toLowerCase()))
      }
    }
  }
  // computed: {
  //   searchResult: function() {
  //     if(this.busca == '' || this.busca == ' '){
  //       return this.pokemons
  //     } else {
  //       return this.pokemons.filter(poke => poke.name == this.busca)
  //     }
  //   }
  // }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #fff;
  margin-top: 60px;
  padding: 24px;
}
</style>
