<template>
    <div class="detail">
        <div class="detailView" v-if="show">
            <div v-if="pokemon" class="image">
                <img :src="imageUrl + pokemon.id + '.png'" >
            </div>
            <div v-if="pokemon" class="datos">
                <h2>{{pokemon.name}}</h2>
                <div class="propiedades">
                    <div class="izq">
                        Experiencia Base
                    </div>
                    <div class="der">
                        {{pokemon.base_experience}}
                    </div>
                </div>
                <div class="propiedades">
                    <div class="izq">
                        Altura
                    </div>
                    <div class="der">
                        {{pokemon.height/10}} m
                    </div>
                </div>
                <div class="propiedades">
                    <div class="izq">
                        Peso
                    </div>
                    <div class="der">
                        {{pokemon.weight/10}} kg
                    </div>
                </div>
                <h3>Tipos</h3>
                <div class="tipos">
                    <div class="tipo" v-for="(valor,index) in pokemon.types"
                    :key="valor + index">
                    {{valor.type.name}}
                    </div>
                </div>
                <h3>Habilidades</h3>
                <div class="habilidades">
                    <div class="habilidad" v-for="(valor,index) in pokemon.abilities"
                    :key="valor + index">
                    {{valor.ability.name}}
                    </div>
                </div>
            </div>
            <h2 v-else>Pokemon no encontrado</h2>
            <button class="btnCerrar" @click="closeDetail">Cerrar</button>
        </div>
        <i v-else class="fas fa-spinner fa-spin"></i>
    </div>
</template>

<script>
export default {
    name: "PokemonDetail",
    props:['pokemonUrl','imageUrl'],
    data: () => {
        return{
            show: false,
            pokemon: {}
        }
    },
    methods: {
        fetchData() {
        let req = new Request(this.pokemonUrl);
        fetch(req)
            .then((resp) => {
            if (resp.status === 200) {
                return resp.json();
            }
            })
            .then((data) => {
                this.pokemon = data;
                this.show = true;
            })
            .catch((error) => {
            console.log(error);
            });
        },
        closeDetail(){
            this.$emit('closeDetail');
        }
    },
    created() {
        this.fetchData();
    }
}
</script>

<style scoped>
    .detail{
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        top: 0;
        left: 0;
        padding: 90px 10px 10px;
        width: calc(100% - 20px);
        height: calc(100vh - 20px);
        background-color: rgba(0, 0, 0, 0.7);
        cursor: pointer;
    }

    .detailView{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        position: relative;
        width: 100%;
        max-width: 510px;
        padding: 50px 0 0;
        background-color: white;
        border-radius: 10px;
        box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2),
                    0 10px 10px rgba(0, 0, 0, 0.2);
    }

    .image{
        display:flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        top: -60px;
        width: 120px;
        height: 120px;
        background-color: #203F59;
        border-radius: 50%;
        overflow: hidden;
        box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2),
                    0 10px 10px rgba(0, 0, 0, 0.2);
    }

    h2{
        text-transform: capitalize;
    }

    .datos{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        width: 100%;
        margin-bottom: 40px;
    }

    .propiedades{
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid white;
        margin-bottom: 10px;
    }

    .izq{
        float:left;
    }

    .der{
        float:right;
    }

    h3{
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid white;
    }

    .tipos,.habilidades{
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 90%;
        max-width: 400px;
    }

    .tipo,.habilidad{
        margin:0 10px 10px 0;
        padding: 5px 10px;
        border-radius: 20px;
        color: white;
        font-size: 1rem;
        letter-spacing: 2px;
        text-transform: capitalize;
        word-wrap: none;
        word-break: keep-all;
    }

    .tipo{
        background-color: #203F59;
    }

    .habilidad{
        background-color: #04D98B;
    }

    .btnCerrar{
        outline: none;
        border: none;
        border-radius: 20px;
        background-color: #333;
        color: white;
        padding: 10px 20px;
        margin-top: 20px;
        margin-bottom: 20px;
        font-size: 1.2 rem;
        cursor: pointer;
    }
</style>