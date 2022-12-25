<script setup lang="ts">
defineEmits([`restart`]);
</script>

<template>
  <footer>
    <button @click="$emit(`restart`)">
      Start Again!
    </button>
  </footer>
</template>

<style scoped>
footer {
  display: grid;
  place-items: center;
  padding: 2rem;
}

button {
  border: none;
  cursor: pointer;
  font-size: 2rem;
  font-weight: bold;
  color: red;
  background: gold;
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  transition: all 0.2s ease-in-out;
}

button:hover {
  translate: 0 5px;
}

button:active {
  scale: 0.95;
}
</style>
