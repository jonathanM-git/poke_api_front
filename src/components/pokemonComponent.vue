<template>
    <div class="pokemon-container">
        <h1>Pokedex</h1>
        <!-- Barra de búsqueda -->
        <div class="search-bar">
        <input type="text" v-model="searchQuery" placeholder="Nombre del pokemon Ej. ivysaur" />
        <button @click="searchPokemon">Buscar</button>
        </div>

        <ul v-if="userData.length > 0">
            <li v-for="(pokemon, index) in userData" :key="index">
                <img :src="pokemon.image" alt="Pokemon Image" />
                <div class="pokemon-text">
                    {{ pokemon.name }}
                </div>
            </li>
        </ul>
        <p v-else>Cargando datos...</p>
    </div>
</template>
    
<script setup>
    import { ref, onBeforeMount } from 'vue';
    
    let userData = ref([]);
    
    onBeforeMount(async () => {
        try {
        // Obtener la lista de Pokémon
        const response = await fetch("https://pokeapi.co/api/v2/pokemon/?offset=0&limit=151");
        const data = await response.json();
    
        // Cargar los detalles de cada Pokémon de forma secuencial
        for (const pokemon of data.results) {
            const detailsResponse = await fetch(pokemon.url);
            const details = await detailsResponse.json();
            userData.value.push({
            name: pokemon.name,
            image: details.sprites.front_default,
            });
        }
    
        console.log(userData.value);
        } catch (error) {
        console.error('Error al obtener los Pokémon:', error);
        }
    });
</script>
