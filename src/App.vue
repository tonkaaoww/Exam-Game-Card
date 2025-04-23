<template>
<div class="bg-gradient-to-br from-orange-400 via-orange-200 to-orange-400  min-h-screen py-4">
  <div class="container mx-auto rounded-xl shadow-2xl bg-white ">
    <header class="text-4xl text-center font-bold mt-6 py-6 ">Game Matching-Card</header>
  </div>
  <GameBoard />
</div>
</template>

<script setup lang="ts">
import GameBoard from './components/GameBoard.vue';

</script>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Bungee+Spice&display=swap');
header{
  font-family: "Bungee Spice", sans-serif;
  font-weight: 400;
  font-style: normal;
  color: orange;
}
</style>