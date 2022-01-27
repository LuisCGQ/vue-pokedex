<template>
<h1>Pokédex</h1>
    <input type="text" placeholder="Busca un pokémon" v-model="searchInput">
    <button @click="handleSearch">Buscar</button>
    <br>
    <div v-if="pokemon.name" class="pokemon">
        <div class="pokemon-data">
            <div class="pokemon-name">{{pokemon.name.toUpperCase()}}</div>
            <br>
            <div v-for="(type, i) in pokemon.types" :key="type.type.name">
                <div>Tipo {{i+1}}: {{type.type.name}}</div>
            </div>
            <div>Habilidad: {{pokemon.ability}}</div>

        </div>
        <img class="pokemon-image" :src="artworkUrl" alt="">
    </div>
</template>

<script>

export default {
    name: "MainLayout",

    data(){
        return{
            searchInput: null,
            pokemon: {},
            artworkUrl: null
        }
        
    },

    methods: {
        handleSearch(){
            fetch(`https://pokeapi.co/api/v2/pokemon/${this.searchInput.toLowerCase()}`)
            .then(res=>res.json())
            .then(res=>{
                this.pokemon.name=res.name
                this.pokemon.types=res.types
                this.pokemon.ability=res.abilities[0].ability.name
                this.artworkUrl=res.sprites.other['official-artwork']['front_default']
                console.log(res)
            })
            .catch(err=>console.log(err))
        }
    }
}
</script>

<style scoped>
.pokemon{
    display: flex;
    justify-content: space-around;
    padding-top: 30px;
    margin: 0 auto;
    max-width: 50%;
}

.pokemon-data{
    text-align: start;
}

.pokemon-image{
    max-width: 250px;
}

.pokemon-name{
    font-weight: bold;
}
</style>
