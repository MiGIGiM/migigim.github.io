<template>
    <router-view to="/details/:id">
        <pokemon-details/>
    </router-view>

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