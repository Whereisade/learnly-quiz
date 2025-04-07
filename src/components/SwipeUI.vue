<template>
  <div class="swipe-ui">
    <div class="mb-4 relative">
      <div class="swipe-directions flex justify-between mb-2">
        <div class="text-gray-600 text-sm">
          ← Physical Change
        </div>
        <div class="text-gray-600 text-sm">
          Chemical Change →
        </div>
      </div>

      <div class="card-area relative h-80 flex justify-center items-center">
        <div 
          ref="card"
          @touchstart="startSwipe" 
          @touchmove="onSwipe" 
          @touchend="endSwipe"
          @mousedown="startDrag"
          @mousemove="onDrag"
          @mouseup="endDrag"
          @mouseleave="endDrag"
          :style="{ 
            transform: `translateX(${translateX}px) rotate(${rotation}deg)`,
            opacity: cardOpacity
          }"
          :class="['swipe-card', { 'transition-transform': isTransitioning }]"
          role="button"
          tabindex="0"
          @keydown="onKeyDown"
          @transitionend="handleTransitionEnd"
        >
          <div v-if="currentCardIndex < cards.length" class="card-content" :style="{ backgroundColor: cards[currentCardIndex].color }">
            <div class="card-icon mb-2">
              <img :src="cards[currentCardIndex].icon" :alt="`Icon for ${cards[currentCardIndex].name}`" class="w-12 h-12" />
            </div>
            <div class="card-title text-white font-semibold">
              {{ cards[currentCardIndex].name }}
            </div>
          </div>
        </div>

        <div class="swipe-buttons absolute bottom-0 left-0 right-0 flex justify-between px-8 pb-4">
          <button 
            @click="handleSwipe('left')" 
            class="bg-blue-200 text-blue-700 w-10 h-10 rounded-full flex items-center justify-center shadow-md"
            aria-label="Swipe Left for Physical Change"
          >
            ←
          </button>
          <button 
            @click="handleSwipe('right')" 
            class="bg-blue-200 text-blue-700 w-10 h-10 rounded-full flex items-center justify-center shadow-md"
            aria-label="Swipe Right for Chemical Change"
          >
            →
          </button>
        </div>
      </div>
    </div>

    <div v-if="feedback.visible" class="feedback-message text-center mb-4" :class="feedback.type" role="alert">
      {{ feedback.message }}
    </div>

    <div class="mt-4 text-center">
      <button 
        @click="$emit('complete')" 
        class="bg-blue-600 text-white py-2 px-8 rounded hover:bg-blue-700 flex items-center mx-auto"
        :disabled="currentCardIndex < cards.length"
      >
        Next <span class="ml-2">→</span>
      </button>
    </div>

    <div v-if="showTryAgain" class="mt-4 text-center">
      <button 
        @click="resetCard" 
        class="bg-gray-200 text-gray-700 py-2 px-8 rounded hover:bg-gray-300 flex items-center mx-auto"
      >
        Try Again
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "SwipeUI",
  data() {
    return {
      cards: [
        {
          name: "Burning Paper",
          type: "chemical",
          color: "#38bdf8", 
          icon: "/flame-icon.svg"
        },
        {
          name: "Melting Ice",
          type: "physical",
          color: "#f97316", 
          icon: "/ice-icon.svg"
        },
        {
          name: "Rusting Metal",
          type: "chemical",
          color: "#a855f7", 
          icon: "/rust-icon.svg"
        }
      ],
      currentCardIndex: 0,
      startX: 0,
      currentX: 0,
      translateX: 0,
      rotation: 0,
      threshold: 100,
      isTransitioning: false,
      cardOpacity: 1,
      isDragging: false,
      feedback: {
        visible: false,
        message: "",
        type: ""
      },
      showTryAgain: false
    };
  },
  methods: {
    
    startSwipe(e) {
      if (this.isTransitioning) return;
      this.startX = e.touches[0].clientX;
      this.feedback.visible = false;
    },
    onSwipe(e) {
      if (this.isTransitioning || this.currentCardIndex >= this.cards.length) return;
      this.currentX = e.touches[0].clientX;
      this.updateCardPosition();
    },
    endSwipe() {
      if (this.isTransitioning || this.currentCardIndex >= this.cards.length) return;
      const direction = this.translateX > 0 ? 'right' : 'left';
      if (Math.abs(this.translateX) > this.threshold) {
        this.handleSwipe(direction);
      } else {
        this.resetCardPosition();
      }
    },
    // Mouse events for desktop
    startDrag(e) {
      if (this.isTransitioning) return;
      this.startX = e.clientX;
      this.isDragging = true;
      this.feedback.visible = false;
    },
    onDrag(e) {
      if (!this.isDragging || this.isTransitioning || this.currentCardIndex >= this.cards.length) return;
      this.currentX = e.clientX;
      this.updateCardPosition();
    },
    endDrag() {
      if (!this.isDragging) return;
      this.isDragging = false;
      if (this.isTransitioning || this.currentCardIndex >= this.cards.length) return;
      const direction = this.translateX > 0 ? 'right' : 'left';
      if (Math.abs(this.translateX) > this.threshold) {
        this.handleSwipe(direction);
      } else {
        this.resetCardPosition();
      }
    },
    
    onKeyDown(e) {
      if (e.key === 'ArrowLeft') {
        this.handleSwipe('left');
      } else if (e.key === 'ArrowRight') {
        this.handleSwipe('right');
      }
    },
    
    updateCardPosition() {
      this.translateX = this.currentX - this.startX;
      this.rotation = this.translateX * 0.1;
    },
    
    resetCardPosition() {
      this.isTransitioning = true;
      this.translateX = 0;
      this.rotation = 0;
      
      setTimeout(() => {
        this.isTransitioning = false;
      }, 300);
    },
    
    handleSwipe(direction) {
      this.isTransitioning = true;
      const card = this.cards[this.currentCardIndex];
      const correctDirection = card.type === 'chemical' ? 'right' : 'left';
      if (direction === correctDirection) {
        
        this.translateX = direction === 'right' ? window.innerWidth : -window.innerWidth;
        this.rotation = direction === 'right' ? 30 : -30;
        this.cardOpacity = 0;
        this.showFeedback("Correct!", "success");
        this.showTryAgain = false;
        setTimeout(() => {
          this.currentCardIndex++;
          this.resetCard();
          if (this.currentCardIndex >= this.cards.length) {
            this.showFeedback("Great Job!", "success");
          }
        }, 500);
      } else {
        
        this.translateX = direction === 'right' ? 40 : -40;
        this.rotation = direction === 'right' ? 5 : -5;
        this.showFeedback("Try again! Swipe based on the type of change.", "error");
        this.showTryAgain = true;
        
        setTimeout(() => {
          this.resetCard();
        }, 500);
      }
    },
    
    resetCard() {
      this.isTransitioning = true;
      this.translateX = 0;
      this.rotation = 0;
      this.cardOpacity = 1;
      setTimeout(() => {
        this.isTransitioning = false;
        this.showTryAgain = false;
      }, 300);
    },
    
    showFeedback(message, type) {
      this.feedback = {
        visible: true,
        message,
        type
      };
      if (type === "error") {
        setTimeout(() => {
          this.feedback.visible = false;
        }, 3000);
      }
    },
    
    handleTransitionEnd() {
      this.isTransitioning = false;
    }
  }
};
</script>

<style scoped>
.swipe-card {
  width: 240px;
  height: 320px;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
  position: absolute;
  will-change: transform;
}

.card-content {
  width: 100%;
  height: 100%;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.transition-transform {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.feedback-message {
  padding: 10px;
  border-radius: 8px;
  font-weight: 500;
}

.feedback-message.success {
  background-color: rgba(34, 197, 94, 0.1);
  color: rgb(34, 197, 94);
}

.feedback-message.error {
  background-color: rgba(239, 68, 68, 0.1);
  color: rgb(239, 68, 68);
}


@media (max-width: 640px) {
  .swipe-card {
    width: 200px;
    height: 280px;
  }
}
</style>
