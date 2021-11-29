<template>
  <div id="party">
    <h4>Party: {{ partyPokemon.length }} of 6</h4>
    <div class="party-pokemon">
      <!-- Party pokemon go here -->
      <PokemonCard
        v-for="p in partyPokemon"
        :key="p"
        :pokemon="p"
        @card-click="removeFromParty(p)"
      />
    </div>
    <div class="party-pokemon" v-show="partyPokemon.length == 0">
      Party is empty. Click on a pokemon to add to your party.
    </div>
  </div>
</template>

<script>
import PokemonCard from "./PokemonCard.vue";

export default {
  name: "PartyPokemon",
  components: {
    PokemonCard,
  },
  props: {
    partyPokemon: Array,
  },
  data() {
    return {
      maxPartySize: 6,
    };
  },
  methods: {
    removeFromParty(pokemon) {
      this.$emit("remove-party", pokemon);
    },
  },
};
</script>

<style scoped>
.party-pokemon {
  display: flex;
  flex-wrap: wrap;
  margin: 0;
}
</style>