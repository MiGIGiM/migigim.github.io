<template>
    <div class="min-h-screen bg-gray-600 flex justify-center items-center">
        <div class="py-12 px-1 bg-white rounded-2xl shadow-xl z-20 grid grid-flow-col grid-cols-2">
            <i class="fas fa-arrow-circle-left absolute z-10 pl-10 text-3xl cursor-pointer" @click="goBack"></i>
            <article>
                <div class="flex justify-center rounded-full m-8 bg-green-100">
                    <img class="h-96" :src="imgUrl" alt="">
                </div>
                <h1 class="text-5xl px-20 font-bold text-center mb-4">{{ pokemon.id + '. ' + capitalizeFirst(pokemon.name) }}</h1>
                <div class="grid grid-col-2 grid-flow-col justify-evenly pt-2">
                    <h3 :class="a.type.name" class="font-bold  text-xl rounded-full py-1 px-4 text-white" v-for="a in pokemon.types" :key="a">
                        {{ capitalizeFirst(a.type.name) }}
                    </h3>
                </div>
            </article>
            <article class="m-10 ">
                    <div class="bg-gray-100 rounded-lg mb-4">
                        <h2 class="text-2xl ml-5 font-semibold">Description</h2> 
                        <p class="text-justify text-xl w-96 ml-2 p-4">{{ pokemon.description[0] }}</p>
                    </div>
                    <div class="bg-gray-100 rounded-lg mb-4">
                        <h2 class="text-2xl ml-5 mb-2 font-semibold">Stats</h2> 
                        <table class="table-fixed ml-10 mb-4">
                            <tr class="text-xl ml-20" v-for="el in pokemon.stats" :key="el">
                                <td>{{ capitalizeFirst(el.stat.name).replace("-", " ") + ': ' }}</td>
                                <td class="pl-2 ">{{ el.base_stat }}</td>
                            </tr>
                        </table>
                    </div>
                    <div class="bg-gray-100 rounded-lg mb-4">
                        <h2 class="text-2xl ml-5 pt-2 font-semibold">Abilities</h2>
                        <ol class="list-decimal list-inside">
                            <li class="text-xl py-1 px-4" v-for="a in pokemon.abilities" :key="a">
                                {{ capitalizeFirst(a.ability.name).replace("-", " ") + ' ' + isHidden(a.is_hidden) }}
                            </li>
                        </ol>
                    </div>
                    
            </article>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default({
    name: 'Pokemon',
    props: [ 'id' ],
    data (){
        return{
            url: 'https://pokeapi.co/api/v2/pokemon/',
            imgUrl: 'https://raw.githubusercontent.com/HybridShivam/Pokemon/master/assets/images/',
            pokemon: {
                id:   ' ',
                img: ' ',
                name: ' ',
                types: ' ',
                abilities: ' ',
                base: ' ',
                stats: ' ',
                speciesUrl: '',
                description: []
            }
        }
    },
    methods:{
        goBack() {
            this.$router.push({ path: `/` })
        },
        fetchData(){
            let pokemonUrl = this.url + this.id
            axios.get(pokemonUrl).then( res => {
                    this.pokemon.id =  res.data.id
                    this.pokemon.img = res.data.sprites.front_default,
                    this.pokemon.name = res.data.name
                    this.pokemon.types = res.data.types
                    this.pokemon.abilities = res.data.abilities
                    this.pokemon.base_exp = res.data.base_experience
                    this.pokemon.stats = res.data.stats
                    this.pokemon.speciesUrl = res.data.species.url
                    this.fetchDesc()
            }).catch((err) => {
                console.log(err);
            });
        },
        fetchDesc(){
            axios.get(this.pokemon.speciesUrl).then( res =>{
                res.data.flavor_text_entries.forEach( el =>{
                    if(el.language.name == "en"){
                        this.pokemon.description.push(Object.values(el)[0])
                    }
                })
            }).catch(err => {
                console.log(err);
            })
        },
        randomDesc(list){

        },
        capitalizeFirst(word){
            return word.charAt(0).toUpperCase() + word.slice(1);
        },
        getOfficialImg(){
            let png = ''
            if(this.id <= 9)
                png = '00' + this.id + '.png'
            
            else if(this.id <= 99)
                png = '0' + this.id + '.png'
            else
                png = this.id +'.png'
            
            this.imgUrl += png
        },
        isHidden(flag){
            return (flag ? "(Hidden Ability)" : ' ');
        }
    },
    created(){
        this.fetchData()
        this.getOfficialImg()
    },
    mounted(){
    }
})
</script>

<style scoped>
.true{
    color: white;
}
</style>