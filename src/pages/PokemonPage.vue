<template>

  <h1 v-if="!pokemon" >Espere por favor...</h1>

  <div v-else>
    <h1>¿Quién es este pokémon?</h1>

    <PokemonPicture 
      :pokemonId="pokemon.id" 
      :showPokemon="showPokemon" />
    <PokemonOptions 
      :pokemons="pokemonArr" 
      @selection="checkAnswer"/>

    <template v-if="showAnswer"  >
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame" class="newGame">
        Nuevo Juego
      </button>
    </template>
  </div>


</template>
<script>
  import PokemonPicture from '@/components/PokemonPicture.vue'
  import PokemonOptions from '@/components/PokemonOptions.vue'

  import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
  components: {
    PokemonPicture,
    PokemonOptions
  },
  data(){
    return{
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  },
  methods:{
    async mixPokemonArray(){
      this.pokemonArr = await getPokemonOptions()

      const rndInt = Math.floor( Math.random() * 4 )
      this.pokemon = this.pokemonArr[ rndInt ]
    },
    checkAnswer( selectedId ){
      this.showPokemon = true
      this.showAnswer = true

      if(selectedId === this.pokemon.id){
        this.message = `Correcto, ${this.pokemon.name}`
      }else{
        this.message = `Oops, era ${this.pokemon.name}`
      }
    },
    newGame(){
      this.showPokemon = false
      this.showAnswer = false
      this.pokemonArr = []
      this.pokemon = null
      this.mixPokemonArray()
    }
  },
  mounted(){
    this.mixPokemonArray()
  }
}
</script>

<style>
  .newGame{
  background-color: white;
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.2);
    cursor: pointer;
    margin-bottom: 10px;
    width: 250px;
    margin-right: -25px;
  }
</style>