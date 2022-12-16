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
    <img
      v-for="avatar, index in randomAvatars"
      :key="index"
      :src="avatar"
      alt=""
    />
  </main>
</template>

