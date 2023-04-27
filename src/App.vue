<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/logo1.png">
      <hr>
      <input type="text"  placeholder="Buscar Pokemon Pelo Nome" v-model="busca"  class="input is-rounded">
      <button class="button is-info is-rounded" id="btnBuscar" @click="buscar" >Buscar</button>
      <div v-for="(poke,index) in resultadoBusca" :key="index">
      <ComponentePokemon :name="poke.name" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios';

import ComponentePokemon from './components/ComponentePokemon.vue';

export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      busca:'',
    }
  },
  created: function(){
   axios.get("https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0").then (res =>{
    console.log("Pegou a lista de Pokemons");
    this.pokemons = res.data.results;
    this.filteredPokemons = res.data.results
   })
  },
  components: {
    ComponentePokemon
  },

  methods:{
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);

      }
  }
},

  computed:{
    
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
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

#btnBuscar{
  width: 100%;
  margin-top: 2%;
}
</style>
