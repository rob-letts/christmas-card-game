<script setup lang="ts">
import ana from '@/assets/ana.png';
import jack from '@/assets/jack.png';
import elsa from '@/assets/elsa.png';
import spidey from '@/assets/spidey.png';
import batman from '@/assets/batman.png';

const avatars = [ana, jack, elsa, spidey, batman, ana, jack, elsa, spidey, batman];
const randomAvatars = [];

while (avatars.length) {
  const numberOutOfAHat = Math.floor(Math.random() * avatars.length);
  const theChosenOne = avatars.splice(numberOutOfAHat, 1);
  randomAvatars.push(theChosenOne);
}
</script>

<template>
  <main>
    <button v-for="avatar, index in randomAvatars">
      <img
        :key="index"
        :src="avatar"
        alt=""
      />
    </button>
  </main>
</template>

<style scoped>
button {
  border: none;
  background: none;
  cursor: pointer;
}

main {
  width: 100vw;
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  flex-wrap: wrap;
  max-width: 750px;
  margin: 0 auto;
}

button {
  flex-basis: 20%;
}
</style>