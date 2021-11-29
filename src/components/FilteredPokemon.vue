<template>
  <div class="mt-2" id="all">
    <h4>Search Results:</h4>
    <div class="poke-container" v-show="filteredPokemon.length==0">No results.</div>
    <div class="poke-container">
      <!-- Pokemon go here -->
      <PokemonCard
        v-for="p in filteredPokemon"
        :key="p"
        :pokemon="p"
        @card-click="addToParty(p)"
      />
    </div>
  </div>
</template>

<script>
import PokemonCard from "./PokemonCard.vue";

export default {
  name: "AllPokemon",
  components: {
    PokemonCard,
  },
  props: {
    filteredPokemon: Array,
  },
  methods: {
    addToParty(pokemon) {
      console.log(`Add ${pokemon.name} to party`);
      this.$emit("add-party", pokemon);
    },
  },
};
</script>

<style scoped>
h4 {
  font-weight: bold;
}

.poke-container {
  display: flex;
  flex-wrap: wrap;
  align-items: space-between;
  justify-content: center;
  margin: 0 auto;
  max-width: 1200px;
}

#all {
  margin: 10px;
}
</style>