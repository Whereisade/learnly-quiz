<template>
  <div class="states-of-matter">
    <div class="grid grid-cols-2 gap-4">
      <div 
        v-for="(card, index) in cards" 
        :key="index" 
        @click="flipCard(index)" 
        @keydown.enter="flipCard(index)" 
        @keydown.space.prevent="flipCard(index)"
        class="card-container"
        role="button"
        tabindex="0"
        :aria-pressed="card.flipped.toString()"
      >
        <div class="card" :class="{ 'flipped': card.flipped }">
          <div class="card-front" :style="{ backgroundColor: card.color }">
            <div class="card-icon">
              <img :src="card.icon" :alt="`${card.name} icon`" class="w-12 h-12" />
            </div>
            <div class="mt-2 text-white font-semibold">{{ card.name }}</div>
          </div>
          <div class="card-back bg-white">
            <div class="p-4 text-center">
              <h3 class="font-bold mb-2">{{ card.name }}</h3>
              <p class="text-sm">{{ card.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="mt-8 text-center">
      <button 
        @click="completeLesson" 
        class="bg-blue-600 text-white py-2 px-8 rounded hover:bg-blue-700 flex items-center mx-auto"
        :disabled="!allCardsFlipped"
      >
        Next <span class="ml-2">→</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TapToReveal",
  data() {
    return {
      cards: [
        {
          name: "Gas",
          description: "A gas expands to fill any container. Its particles move rapidly and spread apart.",
          color: "#e2e8f0",
          icon: "/gas-icon.svg",
          flipped: false
        },
        {
          name: "Liquid",
          description: "A liquid takes the shape of its container but keeps its volume. Its particles are close together.",
          color: "#a855f7",
          icon: "/liquid-icon.svg",
          flipped: false
        },
        {
          name: "Solid",
          description: "Fixed shape & volume. Its particles vibrate but don't move freely.",
          color: "#f97316",
          icon: "/solid-icon.svg",
          flipped: false
        },
        {
          name: "Lightning",
          description: "A form of electricity resulting from the discharge between clouds.",
          color: "#3b82f6",
          icon: "/lightning-icon.svg",
          flipped: false
        }
      ],
    };
  },
  computed: {
    allCardsFlipped() {
      return this.cards.every(card => card.flipped);
    }
  },
  methods: {
    flipCard(index) {
      this.cards[index].flipped = !this.cards[index].flipped;
    },
    completeLesson() {
      if (this.allCardsFlipped) {
        this.$emit('complete');
      }
    }
  },
};
</script>

<style scoped>
.card-container {
  perspective: 1000px;
  height: 180px;
  outline: none;
  
}

.card {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.6s;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  
}

.card.flipped {
  transform: rotateY(180deg);
}

.card-front, .card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.card-back {
  transform: rotateY(180deg);
}

.card-icon {
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
