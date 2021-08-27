<template>
    <div @click="alert" class="box-content border rounded m-2 h-80 w-64 shadow-2xl justify-center grid hover:shadow-none hover:bg-gray-900 hover:text-white hover:border-gray-900 cursor-pointer bg-white">
        <img class="w-64 justify-self-center" :src="pokemon.img">
        <div class="-mt-8">  
            <h1 class="font-bold text-center text-xl">{{ toUppercase(pokemon.name) }}</h1>
            <div class="grid grid-col-2 grid-flow-col justify-around pt-2">
                <h3 :class="a.type.name" class="font-bold  text-xl rounded-full py-1 px-4 text-white" v-for="a in pokemon.types" :key="a">
                    {{ toUppercase(a.type.name) }}
                </h3>
            </div>
        </div>
    </div>
    
</template>

<script>

export default {
    props:{
        pokemon: {},
    },
    data(){
        return{
            selectedPokemon: {},
            showModal: 'hidden'
        }
    },
    methods:{
        alert(){
            this.$router.push({ path: `/details/${this.pokemon.id}` })
        },
        toUppercase(word){
            return word.charAt(0).toUpperCase() + word.slice(1);
        }
    },
    computed: {
        
    }
    
}
</script>

<style>
.bug{
    background-color: #3D9953;
}
.fire{
    background-color: red;
}
.grass{
    background-color: #2CCB56;
}
.flying{
    background-color: #94B2C6;
}
.water{
    background-color: #2551DE;
}
.electric{
    background-color:#F4BD37;
}
.dark{
    background-color: #5B5978;
}
.dragon{
    background-color: #65CAD8;
}
.fighting{
    background-color:#984029;
}
.ghost{
    background-color: #90678F;
}
.fairy{
    background-color:#E80C6A;
}
.steel{
    background-color: #5F756D;
}
.ground{
    background-color:#936A5B;
}
.rock{
    background-color:#6A6A53;
}
.psychic{
    background-color: #F61690;
}
.normal{
    background-color:#C998A7;
}
.poison{
    background-color:#9C68D6;
}
.ice{
    background-color: #89D2F3;
}
</style>