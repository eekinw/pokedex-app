<script setup>
defineProps(["name", "types", "abilities", "images"])

const capitalize = (str) => str.charAt(0).toUpperCase() + str.slice(1);


</script>



<template>
        <div class="flex flex-col justify-center items-center text-center h-auto bg-gray-100 p-4 rounded-lg shadow-md">
        <img :src="images" alt="pokemonSprites">
        <h2 class="font-bold text-2xl mb-2">{{ capitalize(name) }}</h2>
        <p class="text-lg mb-1">{{ types.map(capitalize).join(', ') }}</p>
        <p class="text-lg">{{ abilities.map(capitalize).join(', ') }}</p>
      </div>



      
  </template>