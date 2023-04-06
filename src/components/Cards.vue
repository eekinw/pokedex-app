<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
import Card from "./Card.vue";

const pokemons = ref([]);

async function fetchData() {
    try {
    // This line sends an API request to get the list of the first 151 Pokémon. 
    // The response contains basic information about each Pokémon, such as their name and individual URL.
    const response  = await axios.get("https://pokeapi.co/api/v2/pokemon/?limit=151");
    // This line extracts the individual URLs for each Pokémon from the API response and stores them in 
    // an array called pokemonUrls.
    const pokemonUrls = response.data.results.map(p => p.url);


    // This line maps over the pokemonUrls array and sends an API request for each individual Pokémon URL. 
    // It creates an array of promises, where each promise represents an API request to fetch the details of a specific Pokémon.
    const pokemonDetailsPromises = pokemonUrls.map(url => axios.get(url));

    // This line uses Promise.all() to wait for all the promises in the pokemonDetailsPromises array to resolve. Once all the promises are resolved, 
    // an array of responses (pokemonDetailsResponses) is created. Each response in this array contains the detailed information about a specific Pokémon, such as their types and abilities.
    const pokemonDetailsResponses = await Promise.all(pokemonDetailsPromises);

    // pokemons.value = pokemonDetailsResponses.map(response => { ... });:
    // This line maps over the pokemonDetailsResponses array and extracts the relevant data from each response. For each response, the following steps are performed:
    pokemons.value = pokemonDetailsResponses.map(response => {
      const p = response.data;
      return {
        name: p.name,
        types: p.types.map(t => t.type.name),
        abilities: p.abilities.map(a => a.ability.name),
        images: p.sprites.front_default
      };
    });
  } catch (error) {
    console.log(error);
  }
}



onMounted(fetchData);

</script>

<template>
    <div class="container mx-auto p-4 max-w-screen-lg">
        <h1 class=" text-2xl font-bold">All Original 151 Pokemon</h1>

    <!-- Insert Pokemon Cards ()-->
    <div class="grid grid-cols-3 sm:grid-cols-2 lg:grid-cols-3 gap-8 p-4 bg-white">
        <Card 
        v-for="(pokemon, index) in pokemons"
        :key="index"
        :name="pokemon.name"
        :types="pokemon.types"
        :abilities="pokemon.abilities"
        :images="pokemon.images"
        />
      </div>
    </div>

</template>


<!-- Here's a summary of the process:

The initial axios.get() request fetches the basic information (name and URL) for each Pokémon.
You create a new array called pokemonUrls by extracting the URLs from the response.
You send individual API requests for each Pokémon using the URLs in the pokemonUrls array. These requests return detailed information about each Pokémon, including their types and abilities.
You use Promise.all() to wait until all the promises are resolved. It returns an array called pokemonDetailsResponses that contains the detailed information about each Pokémon.
You map over the pokemonDetailsResponses array to extract the relevant data (name, types, abilities, and images) and store it in an object. The map() function creates a new array of objects with this information, which is then assigned to pokemons.value.
This process allows you to fetch detailed information for each Pokémon by making multiple API requests and then combining the results into a single array of objects. -->