<script setup lang="ts">
import { computed, ref } from 'vue';
import RestartButton from "@/components/RestartButton.vue";
import ana from '@/assets/ana.png';
import jack from '@/assets/jack.png';
import elsa from '@/assets/elsa.png';
import spidey from '@/assets/spidey.png';
import batman from '@/assets/batman.png';
import olaf from '@/assets/olaf.png';

type Avatar = { name: string, src: string, selected: boolean, pair: boolean };

const win = computed(() => {
  return cards.value.every(card => card.pair);
});

const cards = ref<Avatar[]>([]);
const selectedCards = computed(() => {
  return cards.value.filter(card => card.selected).map(card => card.name);
});

const currentSelection = ref<Avatar>();

function setupCards (avatars: string[]) {
  const localAvatars = [...avatars];
  cards.value = [];

  while (localAvatars.length) {
    const numberOutOfAHat = Math.floor(Math.random() * localAvatars.length);
    const theChosenOne = localAvatars.splice(numberOutOfAHat, 1)[0];

    cards.value.push({
      name: theChosenOne.toString().replace(/\/src\/assets\//, ``).replace(/\.png/, ``),
      src: theChosenOne,
      selected: false,
      pair: false
    });
  }
}

function resetCards (avatars: string[]) {
  cards.value.forEach(card => {
    card.selected = false;
    card.pair = false;
  });

  setTimeout(() => setupCards(avatars), 1000);
}

function selectCard (avatar: Avatar) {
  if (selectedCards.value.length < 2) {
    avatar.selected = true;

    // there is another card selected
    if (currentSelection.value) {
      // and its a match
      if (currentSelection.value.name === avatar.name) {
        currentSelection.value.pair = true;
        currentSelection.value.selected = false;

        avatar.pair = true;
        avatar.selected = false;

        currentSelection.value = undefined;
      } else {
        setTimeout(() => {
          if (currentSelection.value) {
            currentSelection.value.selected = false;
            avatar.selected = false;
            currentSelection.value = undefined;
          }
        }, 1500);
      }
    } else {
      currentSelection.value = avatar;
    }
  }
}

const avatars = [ana, jack, elsa, spidey, batman, ana, jack, elsa, spidey, batman];
setupCards(avatars);
</script>

<template>
  <main>
    <div
      v-if="win"
      class="win-message"
    >
      <p>OMG RIVER YOU WON YAY!!!!</p>
      <img
        :src="olaf"
        alt="some avatar"
        id="olaf"
      />
    </div>
    <template v-else>
      <button
        v-for="avatar, index in cards"
        @click="selectCard(avatar)"
        :class="[{ selected: avatar.selected }, { pair: avatar.pair }]"
        :key="index"
      >
        <img
          :src="avatar.src"
          alt="some avatar"
        />
      </button>
    </template>
  </main>
  <RestartButton @click="resetCards(avatars)" />
</template>

<style scoped>
main {
  perspective: 500px;
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

img {
  backface-visibility: hidden;
}

button {
  --shadow-color: 229deg 74% 40%;

  box-shadow:
    0.3px 0.5px 0.7px hsl(var(--shadow-color) / 0.36),
    0.8px 1.6px 2px -0.8px hsl(var(--shadow-color) / 0.36),
    2.1px 4.1px 5.2px -1.7px hsl(var(--shadow-color) / 0.36),
    5px 10px 12.6px -2.5px hsl(var(--shadow-color) / 0.36);
  padding: 1rem;
  background: red;
  border-radius: 10%;
  border: black 3px solid;
  cursor: pointer;
  flex-basis: 20%;
  position: relative;
  transition: all 1s;
  transform-style: preserve-3d;
  rotate: y 180deg;
}

.selected {
  rotate: y 0deg;
  background: whitesmoke;
}

.pair {
  rotate: y 0deg;
  border: 4px solid gold;
  background: whitesmoke;
}

.win-message {
  display: flex;
  gap: 2rem;
  flex-direction: column;
  font-size: 1.5rem;
}

#olaf {
  max-height: 250px;
  border-radius: 8px;
  border: 4px solid gold;
}
</style>