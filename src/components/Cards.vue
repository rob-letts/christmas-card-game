<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import RestartButton from "@/components/RestartButton.vue";
import ana from '@/assets/ana.png';
import jack from '@/assets/jack.png';
import elsa from '@/assets/elsa.png';
import spidey from '@/assets/spidey.png';
import batman from '@/assets/batman.png';
import olaf from '@/assets/olaf.png';
import ethan from '@/assets/ethan.png';
import bobby from '@/assets/bobby.png';
import frankie from '@/assets/frankie.png';
import charlotte from '@/assets/charlotte.png';

type Avatar = { name: string, src: string, selected: boolean, pair: boolean };
const avatars = [ana, jack, elsa, spidey, batman, ethan, bobby, frankie, charlotte];

const cards = ref<Avatar[]>([]);
const currentSelection = ref<Avatar>();
const displayWinMessage = ref<boolean>(false);

const winCondition = computed(() => cards.value.every(card => card.pair));
const selectedCards = computed(() => cards.value.filter(card => card.selected).map(card => card.name));

watch(winCondition, shesTotallyWon => {
  if (shesTotallyWon) {
    setTimeout(() => {
      displayWinMessage.value = true;
    }, 1000);
  }
});

setupCards(avatars);

function setupCards (avatars: string[]) {
  const localAvatars = [...avatars];
  const randomAvatars = [];
  const totalPairs = 5;

  for (let i = 0; i < totalPairs; i++) {
    const numberOutOfAHat = Math.floor(Math.random() * localAvatars.length);
    const theChosenOne = localAvatars.splice(numberOutOfAHat, 1)[0];
    randomAvatars.push(theChosenOne);
  }

  const avatarPairs = [...randomAvatars, ...randomAvatars].sort(() => Math.random() - 0.5);

  cards.value = avatarPairs.map(item => {
    return {
      name: item.toString().replace(/\/src\/assets\//, ``).replace(/\.png/, ``),
      src: item,
      selected: false,
      pair: false
    };
  });
}

function resetCards (avatars: string[]) {
  displayWinMessage.value = false;

  cards.value.forEach(card => {
    card.selected = false;
    card.pair = false;
  });

  setTimeout(() => setupCards(avatars), 1000);
}

function selectCard (avatar: Avatar) {
  if (selectedCards.value.length < 2) {
    avatar.selected = true;

    if (currentSelection.value) {
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
</script>

<template>
  <div
    v-if="displayWinMessage"
    class="win-message"
  >
    <p>OMG RIVER YOU WON YAY!!!!</p>
    <img
      :src="olaf"
      alt="some avatar"
      id="olaf"
    />
  </div>
  <main v-else>
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
  </main>
  <RestartButton @click="resetCards(avatars)" />
</template>

<style scoped>
main {
  perspective: 1000px;
  width: 100vw;
  padding: 2rem;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: 230px 230px;
  gap: 1rem;
  max-width: 1000px;
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
  border-radius: 1rem;
  border: black 3px solid;
  cursor: pointer;
  flex-basis: 22%;
  position: relative;
  transition: all 1s;
  transform-style: preserve-3d;
  rotate: y 180deg;
}

p {
  text-align: center;
}

#olaf {
  max-height: 50vh;
  border-radius: 8px;
  border: 4px solid gold;
  max-width: 50vw;
  margin: 0 auto;
}

.selected {
  rotate: y 0deg;
  background: whitesmoke;
  pointer-events: none;
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
</style>