<script setup lang="ts">
import Title from "@/components/Title.vue";
import Cards from "@/components/Cards.vue";
import RestartButton from "@/components/RestartButton.vue";
</script>

<template>
  <Title />
  <Cards />
  <RestartButton />
</template>

<style>
#app {
  display: grid;
  grid-template-rows: 1fr 5fr 1fr;
  height: 100vh;
  background-color:hsla(198,100%,50%,1);
  background-image: radial-gradient(at 80% 0%, hsla(189,100%,56%,1) 0px, transparent 50%), radial-gradient(at 80% 100%, hsla(242,100%,70%,1) 0px, transparent 50%);
}
</style>