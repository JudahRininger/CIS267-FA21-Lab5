<template>
  <nav>NAVIGATION</nav>

  <PartyPokemon 
    :partyPokemon="pp"
    @remove-pokemon="remove"
    />
    <AllPokemon 
    :partyPokemon="allPokemon"
    @remove-pokemon="addPokemonToParty"
    @add-more="addMore"
    />

  <footer> &copy; 2021 </footer>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import PartyPokemon from "./components/PartyPokemon.vue"
import AllPokemon from "./components/AllPokemon.vue"

export default {
  name: 'App',
  components: {
    PartyPokemon,
    AllPokemon
  },
  data() {
    return {
      pp: [],
      allPokemon: [],
      pokemonCount: 100,
      maxPartySize: 6,
    }
  },
  methods: {
    remove(pokemon) {
      this.pp = this.pp.filter( p => p.guid!= pokemon.guid);
    },

    async loadPokemon() {
      // load all pokemon from API and save into all pokemon
      let pokemon = [];
      for (let i = 1; i <= this.pokemonCount; i++) {
        let p = await this.getPokemon(i);
        if (p.name === "mr-mime") {
          p.name = "Mr. Mime";
        }
        p.isFavorite = false;

        pokemon.push(p);
      }

      pokemon.forEach( p => {
          this.allPokemon.push(p);
      });
    },
    async getPokemon(id) {
      // get pokemon data from pokeapi
      const url = `https://pokeapi.co/api/v2/pokemon/${id}`;
    
      const response = await fetch(url);
      const data = await response.json();
    
      return data;
      //createPokemonCard( data );
    },
    addPokemonToParty(pokemon) 
    {
      if (this.pp.length  < 6) {
        const pokemonCopy = {...pokemon};
        pokemonCopy.guid = this.getGUID();
        this.pp.push(pokemonCopy);
      }
    },
    removePokemonFromParty(pokemon) {      
      this.partyPokemon = this.partyPokemon
                            .filter( p => p.guid != pokemon.guid);
    },
    pokemonTypeString(pokemon) {
      if (pokemon.types.length > 1) {
        return `${pokemon.types[0].type.name} / ${pokemon.types[1].type.name}`;
      } else {
        return `${pokemon.types[0].type.name}`;
      }
    },
    getGUID() {
      return Math.floor(Math.random()* 1000000);
    },
    toggleParty() {
      if (this.partyString == "▼") {
        this.partyString = "▲";
        this.partyShowing = false;
      }
      else {
        this.partyString = "▼"
        this.partyShowing = true;
      }
    },
    async addMore() {
      // load all pokemon from API and save into all pokemon
      let pokemon = [];
      let oldCount = this.pokemonCount;
      this.pokemonCount += 50;
      for (let i = oldCount+1; i <= this.pokemonCount; i++) {
        let p = await this.getPokemon(i);
        p.isFavorite = false;

        pokemon.push(p);
      }

      pokemon.forEach( p => {
          this.allPokemon.push(p);
      });
    }
  },
  computed: {
    partyLength() {
      return this.partyPokemon.length;
    },
    allPokemonHeight() {
      if (this.partyString == "▼") {
        if (this.partyPokemon.length != 0) {
          return "43vh";
        }
        else {
          return "66.4vh";
        }
      }
      else {
        return "74.4vh"
      }
    }
    
  },
  mounted() {
    this.loadPokemon();
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
