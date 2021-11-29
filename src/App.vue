<template>
  <nav>Navigation</nav>

  <PartyPokemon
    :partyPokemon="party"
    :maxPartySize="6"
    @remove-party="removeFromParty"
  />

  <AllPokemon :allPokemon="all" @add-party="addToParty" />

  <footer>&copy; 2021</footer>
</template>

<script>
import PartyPokemon from "./components/PartyPokemon.vue";
import AllPokemon from "./components/AllPokemon.vue";

export default {
  name: "App",
  components: {
    PartyPokemon,
    AllPokemon,
  },
  data() {
    return {
      party: [],
      all: [],
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
      }
      else {
        alert("Party is full. Remove Pokemon from party to add more.")
      }
    },
    removeFromParty(pokemon) {
      this.party.splice(this.party.indexOf(pokemon), 1);
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
  text-align: center;
}

#app {
  font-family: Mohave, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
