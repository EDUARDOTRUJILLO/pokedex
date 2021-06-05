<template>
<div class="barraBusqueda">
    <form @submit.prevent="setPokemonUrl">
        <input type="text" v-model="pokemonBusqueda">
    </form>
    <i class="fas fa-search" @click="setPokemonUrl"></i>
</div>
</template>

<script>
export default {
    props:['apiUrl'],
    data: () => {
        return{
            pokemonBusqueda: '',
        }
    },
    methods:{
        setPokemonUrl() {
            if(this.pokemonBusqueda !== ''){
                this.$emit('setPokemonUrl', this.apiUrl + this.pokemonBusqueda);
            }
        }
    }
}
</script>

<style>
.searchbar {
    position: relative;
    width: 20%;
    max-width: 510px;
    padding-bottom: 20px;
}
input{
    border: none;
    outline: none;
    border-radius: 20px;
    padding: 10px 40px 10px 10px;
    width: calc(100% - 50px);
    font-size: 1rem;
}
i{
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 1.25rem;
    color: black;
    cursor: pointer;
}
</style>