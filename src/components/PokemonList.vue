<template>
        <div class="flex flex-row flex-wrap justify-center h-screen bg-gray-600">
            <i @click="previous" class="fas fa-chevron-circle-left text-3xl mr-16 self-center hover:text-gray-300 cursor-pointer text-white" ></i>
            <div class="grid grid-flow-row grid-cols-5 grid-rows-2 justify-evenly self-center">
                <pokemon-card v-for="pokemon in pokemonArray" :key="pokemon" :pokemon="pokemon"/>
            </div>
            <i @click="next" class="fas fa-chevron-circle-right text-3xl ml-16 self-center cursor-pointer text-white hover:text-gray-300"></i>
        </div>
</template>

<script>
import PokemonCard from "./PokemonCard.vue";
import axios from "axios";
export default {
    components: {
        PokemonCard,
    },
    data() {
        return {
            pokemonArray: [],
            selectedPokemon: [],
            apiUrl: 'https://pokeapi.co/api/v2/pokemon/?limit=10',
            nextUrl: '',
            currentUrl: '',
            previousUrl : '',
        };
    },
    methods: {
        fetchData(){
            axios.get(this.apiUrl).then( res => {
                this.nextUrl = res.data.next;
                this.previousUrl = res.data.previous;
                res.data.results.forEach(pkmn => {
                    this.fetchPokemon(pkmn)
                })
            }).catch(err => {
                console.log(err);
            })
        },
        fetchPokemon(pokemon){
            axios.get(pokemon.url).then( res => {
                let pokemon = {
                    img: res.data.sprites.front_default,
                    id: res.data.id,
                    name: res.data.name,
                    
                };

                console.log(pokemon);
                this.pokemonArray.push(pokemon);
                this.pokemonArray.sort(function(a, b) {
                    return a.id - b.id;
                });
            }).catch((err) => {
                console.log(err);
            });
        },
        next(){
            if(this.nextUrl === null)
                alert("You already reached the last page")
            else{
                this.pokemonArray = []
                this.apiUrl = this.nextUrl
                this.fetchData()
            }
        },
        previous(){
            if(this.previousUrl === null)
                alert("Can't go back any further")
            else{
                this.pokemonArray = []
                this.apiUrl = this.previousUrl
                this.fetchData()
            }
        },
    },
    created() {
        this.currentUrl = this.apiUrl
        this.fetchData()
    },
};
</script>