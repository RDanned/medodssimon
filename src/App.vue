<template>
  <div id="app">
    <div class="game-container">
      <div class="sections" v-if="!isWrong">
        <game-section
          v-for="(color, index) in sections"
          :color="color"
          :key="index"
          :active-input="index == currentShow"
          :id="index"
          @click="click"
        />
      </div>
      <div class="defeat" v-else>
        <div class="defeat__title">
          You defeated.
        </div>
      </div>

      <dashboard
        @start="start"
        @chooseSeverity="chooseSeverity"
        :round="round"
        :show-btn="!isStarted"
      />
    </div>
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

      if (this.isStarted) {
        if (val) {
          this.clear()
          this.isStarted = false
          clearTimeout(this.isClickedTimer)
        } else {
          if (this.playerSequel.length == this.sequel.length) {
            this.playerSequel = []
            this.round++
            this.restart()
          }
        }
      }

      clearTimeout(this.isClickedTimer)
    }, */
    playerSequel: function() {
      console.log('player seq watch')
      if (this.playerSequel != 0) {
        let isWrong = false

        this.playerSequel.map((item, index) => {
          if (this.sequel[index] != item) isWrong = true
        })

        this.isWrong = isWrong

        console.log('is started')
        console.log(this.isStarted)
        if (this.isStarted) {
          if (isWrong) {
            this.clear()
            this.isWrong = true
            clearTimeout(this.isClickedTimer)
          } else {
            if (this.playerSequel.length == this.sequel.length) {
              this.playerSequel = []
              this.round++
              this.restart()
            }
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
    start: function() {
      this.isStarted = true
      this.clear()
      this.restart()
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
          this.isShowing = false
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
        if (!this.isClicked) {
          this.clear()
          this.isWrong = true
        }
      }, this.severity)
    },
    click: function(id) {
      if (!this.isShowing && this.isStarted) {
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
