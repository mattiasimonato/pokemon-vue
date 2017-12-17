<template>
  <div id="hello">
    <h1>Vue.js loves Pokemons</h1>
    <input v-on:keyup.enter="filterPokemon" placeholder="Type the pokemon and press enter..."/>
    <ul v-if="pokemons.length">
      <li v-for="(pokemon) of pokemons">
      <img v-on:click="removePokemon(pokemon)" class="x-icon" src="https://www.econedlink.org/iat/assets/imgs/deleteSign.png" alt="">
      <img v-bind:src="pokemon.img"/>
        <p><strong>{{pokemon.name}}</strong></p>
      </li>
    </ul>
    <div v-else>... no pokemons found :(</div>
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
    filterPokemon: function(e) {
      if (!e.target.value) this.getPokemon()
      this.pokemons = this.pokemons.filter((pokemon) => {
        return pokemon.name.includes(e.target.value)
      })
    },
    removePokemon: function(pokemon) {
      console.log(pokemon - 1)
      this.pokemons.splice(this.pokemons.indexOf(pokemon), 1);
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
  transition: 0.35s box-shadow ease;
  position: relative;
}

li:hover  {
  box-shadow: 0px 0px 16px 0px rgba(0,0,0,0.4);
}

input {
  border: 0;
  padding: 24px;
  width: 400px;
  font-size: 18px;
  box-shadow: 0px 0px 8px 0px rgba(0,0,0,0.2);
  margin-bottom: 40px;
  outline: none;
  transition: 0.35s box-shadow ease;
}
input:focus {
  box-shadow: 0px 0px 16px 0px rgba(0,0,0,0.4);

}
.x-icon {
  position: absolute;
  right: 0;
  top: 0;
  transform: translate(50%, -50%);
  width: 30px;
  height: auto;
  visibility: hidden;
  opacity: 0;
  transition: 0.35s all ease;
  cursor: pointer;
}

li:hover .x-icon  {
  visibility: visible;
  opacity: 1;
}

</style>
