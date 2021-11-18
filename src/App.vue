<template>
  <nav>NAVIGATION</nav>

  <PartyPokemon 
    :partyPokemon="pp"
    @remove-pokemon="remove"
    />

  <footer> &copy; 2021 </footer>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import PartyPokemon from "./components/PartyPokemon.vue"

export default {
  name: 'App',
  components: {
    PartyPokemon
  },
  data() {
    return {
      pp: [
        {name:"Charmader", id: 3},
        {name:"Bulbasaur", id: 2},
        {name:"Pikachu", id: 4}
        ]
    }
  },
  methods: {
    remove(id) {
      this.pp = this.pp.filter( p => p.id!= id);
    },

    loadAllPokemon() {
      //load all pokemon from API
    }
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
</style>
