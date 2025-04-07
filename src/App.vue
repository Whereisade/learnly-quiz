<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-4xl font-extrabold text-center mb-8 text-white">Science Quiz App</h1>
    
    <div class="quiz-container">
      <div v-if="currentStep === 'tap-to-reveal'" class="mb-8">
        <h2 class="text-2xl font-semibold mb-4 text-primary">Lesson 2.1</h2>
        <p class="mb-4 text-secondary">Tap to Discover States of Matter!</p>
        
        <TapToReveal @complete="goToSwipeUI" :key="quizKey" />
      </div>
      
      <div v-if="currentStep === 'swipe-ui'" class="mb-8">
        <h2 class="text-2xl font-semibold mb-4 text-primary">Lesson 3.1</h2>
        <p class="mb-4 text-secondary">Tap the arrows below the cards to swipe the cards in the direction of the correct change.</p>
        <SwipeUI @complete="onComplete" :key="quizKey" />
      </div>
      
      <div v-if="currentStep === 'complete'" class="text-center">
        <h2 class="text-2xl font-semibold mb-4 text-success">Great Job!</h2>
        <button 
          @click="resetQuiz" 
          class="bg-gradient-to-r from-blue-500 to-purple-600 text-white py-2 px-4 rounded hover:scale-105 transform transition duration-300"
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
  background: linear-gradient(135deg, #ff7eb3, #ff758c, #ffb347, #ffcc33);
  background-size: 400% 400%;
  animation: gradientAnimation 15s ease infinite;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

@keyframes gradientAnimation {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.container {
  text-align: center;
}

.quiz-container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  padding: 20px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.quiz-container:hover {
  transform: scale(1.02);
  box-shadow: 0 6px 25px rgba(0, 0, 0, 0.3);
}

.text-gradient {
  /* background: linear-gradient(90deg, #ff7eb3, #ff7575); */
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.text-primary {
  color: #4c51bf;
}

.text-secondary {
  color: #718096;
}

.text-success {
  color: #48bb78;
}

button {
  cursor: pointer;
  transition: transform 0.3s ease, background-color 0.3s ease;
}

button:hover {
  transform: scale(1.05);
}
</style>
