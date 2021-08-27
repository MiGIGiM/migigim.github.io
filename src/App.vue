<template>
    <div>
        <router-view></router-view>
    </div>
</template>

<script>
import Home from './views/Home.vue'
import PokemonDetails from './views/Pokemon.vue'
export default {
    components:{
        Home,
        PokemonDetails
    }
}
</script>