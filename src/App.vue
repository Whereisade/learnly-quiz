<template>
  <div>
    <TapToReveal />
    <SwipeUI />
  </div>
</template>

<script>
import TapToReveal from './components/TapToReveal.vue'
import SwipeUI from './components/SwipeUI.vue'

export default {
  name: 'App',
  components: {
    TapToReveal,
    SwipeUI
  }
}
</script>
