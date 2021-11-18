<template>
  <div id="party">
    <h2>Party</h2>
    <h3>{{ partyPokemon.length }} of 6</h3>
    <div id="party-pokemon">

      <PokemonCard 
        v-for="p in partyPokemon" 
        :key="p.id" 
        :pokemon="p" 
        
        @click-pokemon="remove"
        />

    </div>
  </div>
</template>

<script>
import PokemonCard from "./PokemonCard.vue"

export default {
  name: "PartyPokemon",
  components: {
    PokemonCard
    },
  props: {
    partyPokemon: Array,
  },
  methods: {
    remove(id) {
      this.$emit("remove-pokemon", id);
    }
  }
};
</script>

<style></style>
