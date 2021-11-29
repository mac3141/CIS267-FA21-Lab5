<template>
  <div class="mt-2" id="party">
    <h4>
      Party: {{ partyPokemon.length }} of 6
      <button
        type="button"
        class="btn btn-secondary btn-sm"
        v-show="!partyPokemonIsEmpty"
        @click="emitClear"
      >
        Clear
      </button>
    </h4>
    <div class="party-pokemon" id="subParty">
      <!-- Party pokemon go here -->
      <PokemonCard
        id="partyCards"
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
    emitClear() {
      this.$emit("clear-party");
    },
  },
};
</script>

<style scoped>
h4 {
  font-weight: bold;
}

.party-pokemon {
  display: flex;
  flex-wrap: wrap;
  margin: 0;
}

#party {
  margin: 10px;
  min-height: 20vh;
}

#subParty {
  margin: 0;
}

#partyCards {
  transform: scale(0.8);
  align-items: flex-start;
  margin: 0;
}

#partyCards:hover {
  transform: scale(0.84);
}
</style>