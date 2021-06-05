<template>
    <el-container>
        <el-header>
            <div class="titulo">
                <span id="titulo">Pokédex</span>
                <PokemonSearch 
                :apiUrl="apiUrl" 
                @setPokemonUrl="setPokemonUrl" />
            </div>
        </el-header>

        <PokeList 
            :imageUrl="imageUrl" 
            :apiUrl="apiUrl" 
            @setPokemonUrl="setPokemonUrl"/>

        <PokemonDetail 
            v-if="showDetail"
            :pokemonUrl = "pokemonUrl"
            :imageUrl = "imageUrl"
            @closeDetail = "closeDetail"/>
    </el-container>
</template>

<script>
import PokemonSearch from './pokemonSearch.vue'
import PokeList from './PokeList.vue'
import PokemonDetail from './PokemonDetail.vue'

export default{
    name: 'Pokemon',
    data: () =>{
        return{
            imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
            apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
            pokemonUrl: '',
            showDetail: false,
        }
    },
    components:{
        PokemonSearch,
        PokeList,
        PokemonDetail,
    },
    methods:{
        setPokemonUrl(url){
            this.pokemonUrl = url;
            this.showDetail = true;
        },
        closeDetail(){
            this.pokemonUrl = '';
            this.showDetail = false;
        },
    }
}
</script>

<style scoped>

.el-container{
    display: flex;
    flex-direction: column;
    width: 100%;
    min-height: calc(100vh - 20px);
    font-family: 'Rubik',Arial, Helvetica, sans-serif;
    margin: 0px auto;
    padding-top: 0%;
}

.el-header{
    background-color: #DA3323;
    color: white;
    width: 100%;
    margin: 0px auto;
    display: flexbox;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
    justify-content: center;
    align-items: center;
    padding-top: 5px;
}

.el-input{
    width: 30%;
    float: right;
    margin-top: 10px;
    border-radius: 10px;
}

.titulo{
    display:flex;
    font-weight: bold;
    font-size: 32px;
    justify-content: space-between;
    align-items: center;
}
</style>