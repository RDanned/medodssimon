<template>
  <div id="app">
    <div class="sections">
      <game-section
        v-for="(color, index) in sections"
        :color="color"
        :key="index"
        :active-input="index == currentShow"
        :id="index"
        @click="click"
      />
    </div>

    {{ currentShow }}
    {{ sequel }}
    <p>wrong: {{ isWrong }}</p>
    <dashboard
      @start="restart"
      @chooseSeverity="chooseSeverity"
      :round="round"
    />
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
      playerSequel: [],
      currentIndex: 0,
      currentShow: null,
      isStarted: false,
      isShowing: false,
      isWrong: false,
      isClicked: false,
      severity: 1500,
      isClickedTimer: null
    }
  },
  watch: {
    /* isWrong: function(val) {
      console.log('is wrong watch')
      console.log(val)
      if (val) {
        this.clear()
        this.restart()
      } else {
        this.round++
        this.restart()
      }
    }, */
    playerSequel: function() {
      console.log('player seq watch')
      if (this.playerSequel != 0) {
        let isWrong = false

        this.playerSequel.map((item, index) => {
          if (this.sequel[index] != item) isWrong = true
        })

        this.isWrong = isWrong

        if (isWrong) {
          this.clear()
          this.restart()
        } else {
          if (this.playerSequel.length == this.sequel.length) {
            this.playerSequel = []
            this.round++
            this.restart()
          }
        }

        clearTimeout(this.isClickedTimer)
      }

      /* this.isWrong = !(this.sequel[addedIndex] == this.playerSequel[addedIndex])

      if (!(this.sequel[addedIndex] == this.playerSequel[addedIndex])) {
        this.clear()
        this.restart()
      } else {
        this.round++
        this.restart()
      } */
    }
  },
  methods: {
    clear: function() {
      this.round = 1
      this.sequel = []
      this.playerSequel = []
      this.currentIndex = 0
      this.currentShow = null
      this.isStarted = false
      this.isShowing = false
      this.isWrong = false
      this.isClicked = false
    },
    restart: function() {
      console.log('restart')

      this.isStarted = true
      this.sequel.push(this.getNext())

      this.showSequel()
    },
    showSequel: function() {
      let timer = setInterval(() => {
        if (this.currentIndex == this.sequel.length) {
          clearInterval(timer)
          this.currentIndex = 0
          this.isShown = true
          this.currentShow = null
          this.checkClicked()
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
    },
    checkClicked: function() {
      console.log('setted checker clicker')
      this.isClicked = false
      this.isClickedTimer = setTimeout(() => {
        if (!this.isClicked) this.isWrong = true
      }, this.severity)
    },
    click: function(id) {
      if (!this.isShowing) {
        this.isClicked = true
        clearTimeout(this.isClickedTimer)
        this.checkClicked()
        console.log('click')
        this.playerSequel.push(id)
      }
    },
    chooseSeverity: function(speed) {
      console.log('new speed')
      console.log(speed)
      this.severity = speed
    }
  }
}
</script>
