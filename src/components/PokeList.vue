<template>
  <div class="lista">
    <el-card
      class="box-card"
      v-for="(pokemon, index) in pokemons"
      :key="'poke' + index"
      @click="setPokemonUrl(pokemon.url)">

      <div class="card-align">
        <div class="block-img">
          <img class="pokeimg" :src="imageUrl + pokemon.id + '.png'" />
        </div>
        <div class="block-name">
          <h2>{{ pokemon.name }}</h2>
        </div>
      </div>
    </el-card>

    <div id="scroll-trigger" v-infinite-scroll="scrollTrigger" ref="infinitescrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>  
  </div>
</template>

<script>
export default {
  name: "PokeList",
  props: ["imageUrl", "apiUrl"],
  data: () => {
    return {
      pokemons: [],
      nextUrl:'',
      currentUrl:''
    };
  },
  methods: {
    obtenerPokemon() {
      let req = new Request(this.currentUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) {
            return resp.json();
          }
        })
        .then((data) => {
          this.nextUrl = data.next;
          data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function (part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    scrollTrigger(){
      const observer = new IntersectionObserver((entries)=>{
        entries.forEach(entry =>{
          if(entry.intersectionRatio > 0 && this.nextUrl){
            this.next();
          }
        });
      });
      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next(){
      this.currentUrl = this.nextUrl;
      this.obtenerPokemon();
    },
    setPokemonUrl(url){
    this.$emit('setPokemonUrl',url);
    },
  },
  created(){
    this.currentUrl = this.apiUrl;
    this.obtenerPokemon();
  },
  mounted(){
    this.scrollTrigger();
  }
}
</script>

<style scoped>
.lista {
  display: grid;
  grid-template-columns: repeat(3, minmax(0px, 1fr));
  grid-gap: 10px;
  width: calc(100% - 20px);
  margin-top: 2%;
  padding-left: 10px;
}
.box-card {
  display: flex;
  flex-direction: column;
  border-radius: 20px;
  max-height: 20vw;
  margin-top: 10px;
  filter: drop-shadow(0px 2px 3px rgba(0, 0, 0, 0.3));
}
.card-align {
  display: flexbox;
  width: 100%;
  justify-content: center;
  align-items: center;
}
.pokeimg {
  width: 10vw;
  height: 10vw;
}
.block-img {
  float: left;
  max-width: 8vw;
  max-height: 8vw;
  padding-right: 10px;
}
.block-name {
  font-size: 1.5vw;
  padding-left: 1vw;
  margin-left: 10vw;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: tomato;
}
</style>>