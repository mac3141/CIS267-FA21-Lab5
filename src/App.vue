<template>
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <!-- Bootstrap CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3"
      crossorigin="anonymous"
    />

    <title>Pokedex | Vue CLI</title>
  </head>

  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <h1>Pokedex</h1>
      </a>
      <form class="d-flex">
        <input
          class="form-control me-2"
          id="searchInput"
          type="text"
          placeholder="ID, name, or type"
          aria-label="Search"
          @keyup="filterPokemon"
        />
        <button
          class="btn btn-outline-secondary"
          id="searchButton"
          type="button"
          @click="filterPokemon"
        >
          Search
        </button>
      </form>
    </div>
  </nav>

  <PartyPokemon
    :partyPokemon="party"
    :maxPartySize="6"
    @remove-party="removeFromParty"
    @clear-party="clearParty"
  />

  <AllPokemon
    ref="allPokemonEl"
    v-show="searchIsEmpty"
    :allPokemon="all"
    @add-party="addToParty"
  />

  <FilteredPokemon
    v-show="!searchIsEmpty"
    :filteredPokemon="filtered"
    @add-party="addToParty"
  />

  <footer>&copy; 2021</footer>
</template>

<script>
import PartyPokemon from "./components/PartyPokemon.vue";
import AllPokemon from "./components/AllPokemon.vue";
import FilteredPokemon from "./components/FilteredPokemon.vue";

export default {
  name: "App",
  components: {
    PartyPokemon,
    AllPokemon,
    FilteredPokemon,
  },
  data() {
    return {
      party: [],
      all: [],
      filtered: [],
      searchIsEmpty: true,
    };
  },
  methods: {
    async getPokemonData(id) {
      // get an individual pokemon
      const url = `https://pokeapi.co/api/v2/pokemon/${id}`;
      const response = await fetch(url);
      const data = await response.json();

      return data;
    },
    async loadPokemon() {
      // load all Pokemon from API and save to allPokemon
      const pokemon_count = 150;
      const pokemonArray = [];
      for (let i = 1; i <= pokemon_count; i++) {
        let pokemon = await this.getPokemonData(i);
        pokemonArray.push(pokemon);
      }

      pokemonArray.forEach((pokemon) => {
        this.all.push(pokemon);
      });
    },
    addToParty(pokemon) {
      if (this.party.length < PartyPokemon.data().maxPartySize) {
        this.party.push(pokemon);
      } else {
        alert("Party is full. Remove Pokemon from party to add more.");
      }
    },
    removeFromParty(pokemon) {
      this.party.splice(this.party.indexOf(pokemon), 1);
    },
    clearParty() {
      this.party = [];
    },
    filterPokemon() {
      // set filtered to matching pokemon based on search query
      const searchInput = document.getElementById("searchInput");
      const searchQuery = searchInput.value;
      console.log(searchQuery);

      // search by name, id, or type
      this.filtered = this.all.filter((pokemon) => {
        if (searchQuery == "") {
          this.searchIsEmpty = true;
          this.filtered = [];
        } else if (
          !(
            pokemon.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
            pokemon.id.toString().includes(searchQuery) ||
            this.$refs.allPokemonEl.$refs.pokemonCardEl
              .pokemonTypeString(pokemon)
              .includes(searchQuery)
          )
        ) {
          this.searchIsEmpty = false;
        } else if (
          pokemon.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
          pokemon.id.toString().includes(searchQuery) ||
          this.$refs.allPokemonEl.$refs.pokemonCardEl
            .pokemonTypeString(pokemon)
            .includes(searchQuery)
        ) {
          this.searchIsEmpty = false;
          return true;
        }
      });
      console.log(this.filtered);
    },
  },
  mounted() {
    this.loadPokemon();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Mohave:wght@300;400;600;700&display=swap");

footer {
  position: fixed;
  bottom: 10px;
  right: 20px;
}

nav div a h1 {
  letter-spacing: 3px;
  font-weight: bold;
}

#app {
  font-family: Mohave, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
