<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <hr />
      <img src="./assets/pokewikilogo.gif" alt="Logo" />
      <input
        class="input"
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="search"
      />
      <button
        @click="searchFunction()"
        class="button is-fullwidth is-success"
        id="search-btn"
      >
        Buscar
      </button>
      <h4 class="is-size-4">Pokedex</h4>
      <div
        class="block"
        v-for="(pokemon, index) in filteredPokemons"
        :key="pokemon.url"
      >
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: "",
    };
  },
  created: async function () {
    // É chamado quando o componente é criado
    try {
      let dataPokemon = await axios.get(
        `https://pokeapi.co/api/v2/pokemon?limit=151&offset=0`
      );
      this.pokemons = dataPokemon.data.results;
      this.filteredPokemons = dataPokemon.data.results;
    } catch (err) {
      console.log("err");
    }
  },
  methods: {
    searchFunction: function () {
      this.filteredPokemons = this.pokemons;
      if (this.search == "" || this.search == " ") {
        this.filteredPokemon = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.includes(this.search)
        );
      }
    },
  },
  computed: {
    // resultSearch: function () {
    //   if (this.search == "" || this.search == " ") {
    //     return this.pokemons;
    //   } else {
    //     let resultSearch = this.pokemons.filter((poke) => poke.name.includes(this.search));
    //     return resultSearch;
    //   }
    // },
  },
};
</script>

<style>
* {
  text-align: center;
}
.center {
  margin: auto;
}

#search-btn {
  margin-top: 1%;
}
</style>
