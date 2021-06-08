<template>
  <div id="app">
    <div class="sections">
      <game-section
        v-for="(color, index) in sections"
        :color="color"
        :key="index"
        :active-input="index == currentShow"
      />
      <!-- <game-section color="blue" :active-input="check(0)" />
      <game-section color="red" :active-input="check(1)" />
      <game-section color="yellow" :active-input="check(2)" />
      <game-section color="green" :active-input="check(3)" /> -->
    </div>

    {{ currentShow }}
    {{ sequel }}
    <dashboard @start="start" :round="round" />
  </div>
</template>
<script>
import '@/assets/css/style.scss'
import GameSection from '@/components/GameSection'
import Dashboard from '@/components/Dashboard'

export default {
  name: 'App',
  components: {Dashboard, GameSection},
  data() {
    return {
      sections: ['blue', 'red', 'yellow', 'green'],
      round: 1,
      sequel: [],
      currentIndex: 0,
      currentShow: 'no',
      isStarted: false,
      isShown: false
    }
  },
  methods: {
    start: function() {
      console.log('start')

      this.isStarted = true
      this.sequel.push(this.getNext())

      this.showSequel()
    },
    showSequel: function() {
      console.log('show sequel')
      let timer = setInterval(() => {
        console.log('this.currentShow')
        console.log(this.currentShow)

        if (this.currentIndex == this.sequel.length) {
          clearInterval(timer)
          this.currentIndex = 0
          this.isShown = true
          this.currentShow = 'no'
        } else {
          this.playSound()
          this.currentShow = this.sequel[this.currentIndex]
          this.currentIndex++
        }

        setTimeout(() => {
          this.currentShow = null
        }, 300)
      }, 500)
    },
    playSound: function() {
      const sound = new Audio(require('@/assets/audio/beep.wav'))
      sound.play()
    },
    getNext: function() {
      let max = 3
      let min = 0
      return (Math.random() * (max - min) + min).toFixed(0)
    },
    check: function(index) {
      return index == this.sequel[this.currentIndex]
    }
  }
}
</script>
