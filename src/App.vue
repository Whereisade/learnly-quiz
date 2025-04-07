<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-2xl font-bold text-center mb-8">Science Quiz App</h1>
    
    <div class="quiz-container">
      

      <div v-if="currentStep === 'tap-to-reveal'" class="mb-8">
        <h2 class="text-xl font-semibold mb-4">Lesson 2.1</h2>
        <p class="mb-4">Tap to Discover States of Matter!</p>
        
        <TapToReveal @complete="goToSwipeUI" :key="quizKey" />
      </div>
      
      <div v-if="currentStep === 'swipe-ui'" class="mb-8">
        <h2 class="text-xl font-semibold mb-4">Lesson 3.1</h2>
        <p class="mb-4">Tap the arrows below the cards to swipe the cards in the direction of the correct change.</p>
        <SwipeUI @complete="onComplete" :key="quizKey" />
      </div>
      
      <div v-if="currentStep === 'complete'" class="text-center">
        <h2 class="text-xl font-semibold mb-4">Great Job!</h2>
        <button 
          @click="resetQuiz" 
          class="bg-blue-600 text-white py-2 px-4 rounded hover:bg-blue-700"
        >
          Try Again
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import TapToReveal from './components/TapToReveal.vue';
import SwipeUI from './components/SwipeUI.vue';

export default {
  name: 'App',
  components: {
    TapToReveal,
    SwipeUI
  },
  data() {
    return {
      currentStep: 'tap-to-reveal',
      quizKey: 0 
    };
  },
  methods: {
    goToSwipeUI() {
      this.currentStep = 'swipe-ui';
    },
    onComplete() {
      this.currentStep = 'complete';
    },
    resetQuiz() {
      this.quizKey++;
      this.currentStep = 'tap-to-reveal';
    }
  }
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f5f5f5;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  text-align: center;
}

.quiz-container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 20px;
}
</style>
