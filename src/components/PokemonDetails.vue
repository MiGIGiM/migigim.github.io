<template>
        <div>
            <div class="mx-auto my-16 flex pt-5 pb-5 container bg-gray-50 rounded">
                <img :src="imgUrl + pokemon.id + '.png'" class="h-96">
                <div class="cointainer mx-auto">
                    <h1 class="text-5xl font-semibold"> {{pokemon.id + ' - ' + pokemon.name}} </h1>
                    <div class="grid grid-col-2 grid-flow-col justify-evenly pt-2">
                        <h3 :class="a.type.name" class="font-bold  text-xl rounded-full py-1 px-4 text-white" v-for="a in pokemon.types" :key="a">
                            {{ (a.type.name) }}
                        </h3>
                    </div>
                    <p v-for="el in pokemon.stats" :key="el">{{ el.stat.name + '    ' + el.base_stat}}</p>
                </div>
            </div>
        </div>
</template>

<script>
import axios from "axios";

export default({
    data (){
        return{
            url: 'https://pokeapi.co/api/v2/pokemon/ralts',
            imgUrl: 'https://raw.githubusercontent.com/HybridShivam/Pokemon/master/assets/images/',
            pokemon: {
                img: ': ',
                id:   ': ',
                name: ': ',
                type: ': ',
                abilities: ': ',
                base: ': ',
                stat: ': ',
            }
        }
    },
    methods:{

    },
    created(){
        axios.get(this.url).then( res => {
                    this.pokemon.img = res.data.sprites.front_default
                    this.pokemon.id =  res.data.id
                    this.pokemon.name = res.data.name
                    this.pokemon.types = res.data.types
                    this.pokemon.abilities = res.data.abilities
                    this.pokemon.base_exp = res.data.base_experience
                    this.pokemon.stats = res.data.stats
            }).catch((err) => {
                console.log(err);
            });
    }
})
</script>
