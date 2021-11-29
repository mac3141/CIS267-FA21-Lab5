<template>
  <div class="pokemon" :style="pokemonColorStyle(pokemon)" @click="emitClick">
    <div class="img-container">
      <img :src="pokemon.sprites.other['official-artwork'].front_default" />
    </div>
    <div class="info">
      <span class="number">#{{ pokemon.id.toString().padStart(3, "0") }}</span>
      <h3 class="name">{{ pokemon.name }}</h3>
      <small class="type"
        >Type: <span>{{ pokemonTypeString(pokemon) }}</span>
      </small>
    </div>
  </div>
</template>

<script>
const typeColors = {
  fire: "#FEAC72",
  grass: "#B6DA81",
  electric: "#F4E47C",
  water: "#73ADD3",
  ground: "#C6B56C",
  rock: "#D5B834",
  fairy: "#FEEBF9",
  poison: "#D0A9DB",
  bug: "#92BF5F",
  dragon: "#9D75FA",
  psychic: "#F7A1D3",
  flying: "#7BDAF4",
  fighting: "#E59461",
  normal: "#C9CDCF",
  ice: "#93DBF0",
  ghost: "#9E8BBB",
  dark: "#8F8F8F",
  steel: "#C4D3D4",
};

export default {
  name: "PokemonCard",
  props: {
    pokemon: Object,
  },
  methods: {
    emitClick() {
      this.$emit("card-click");
    },
    pokemonColorStyle(pokemon) {
      if (pokemon.types.length > 1) {
        return {
          background: `linear-gradient(30deg, ${
            typeColors[pokemon.types[0].type.name]
          } 50%, ${typeColors[pokemon.types[1].type.name]} 50%)`,
        };
      } else {
        return {
          background: typeColors[pokemon.types[0].type.name],
        };
      }
    },
    pokemonTypeString(pokemon) {
      if (pokemon.types.length > 1) {
        return `${pokemon.types[0].type.name} | ${pokemon.types[1].type.name}`;
      } else {
        return `${pokemon.types[0].type.name}`;
      }
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;800&display=swap");

.pokemon {
  background-color: #eee;
  border-radius: 10px;
  box-shadow: 0 3px 15px rgba(100, 100, 100, 0.5);
  margin: 10px;
  padding: 20px;
  text-align: center;
  transform: scale(1);
  transition: all 0.2s;
  cursor: pointer;
}

.pokemon:hover {
  transform: scale(1.05);
}

.pokemon .img-container {
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  width: 100px;
  height: 100px;
  text-align: center;
  margin: auto;
}

.pokemon .img-container img {
  /* max-width: 90%; */
  width: 120%;
  margin-top: 00px;
  margin: auto;
}

.pokemon .info {
  margin-top: 20px;
}

.pokemon .info .number {
  background-color: rgba(0, 0, 0, 0.1);
  padding: 5px 10px;
  border-radius: 10px;
  font-size: 0.8em;
}

.pokemon .info .name {
  font-family: "Playfair Display", serif;
  font-weight: 400;
  margin: 15px 0 7px;
  letter-spacing: 1px;
  text-transform: capitalize;
}

.pokemon .info small {
  font-size: 0.7em;
  text-transform: capitalize;
  font-weight: 300;
}
</style>