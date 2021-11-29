<template>
  <div id="party">
    <div class="sticky">
      <h2>Party: <span class="smallText">{{ partyPokemon.length }}/6 <button @click="toggleParty" class="arrow">{{partyString}}</button></span></h2>
    </div>  
    <div class="pokemon-outer" v-if="partyShowing" id="party-pokemon" :key="partyKey">
      <PokemonCard 
        class="width"
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
  data() {
    return {
      partyString: "▲",
      partyShowing: false,
    }
  },
  methods: {
    remove(pokemon) {
      this.$emit("remove-pokemon", pokemon);
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
  }
};
</script>

<style>
.pokemon-outer {
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-basis: 15%;
  border-radius: 10px;
  margin: 10px;
  padding: 20px;
  text-align: center;
  transform: scale(1);
  transition: all 0.2s;
  cursor: pointer;
  overflow: scroll;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.width {
  max-width: 19vh;
}
  </style>
