<template>
  <div id="hello">
    <h1>Vue.js loves Pokemons</h1>
    <ul v-if="pokemons && pokemons.length">
      <li v-for="(pokemon) of pokemons">
      <img v-bind:src="pokemon.img"/>
        <p><strong>{{pokemon.name}}</strong></p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    console.log(this)
    return {
      pokemons: []
      }
  },
  methods: {
    getPokemon: function () {
      axios.get(`https://pokeapi.co/api/v2/pokemon/?limit=151`)
      .then(response => {
        // JSON responses are automatically parsed.
        this.pokemons = response.data.results;
        let id = 0;
        this.pokemons.forEach((obj) => {
          id++
          obj.id = id;
          obj.img = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
        })
        console.log("pokemon array", this.pokemons)
      })
    },
  },
  created() {
    this.getPokemon()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

}
li {
  margin: 20px;
  padding: 20px 40px;
  box-shadow: 0px 0px 8px 0px rgba(0,0,0,0.2);
}

</style>
