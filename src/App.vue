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
    playerSequel: function() {
      console.log('player seq watch')
      if (this.playerSequel != 0) {
        let isWrong = false

        this.playerSequel.map((item, index) => {
          if (this.sequel[index] != item) isWrong = true
        })

        this.isWrong = isWrong

        if (this.isStarted) {
          if (isWrong) {
            this.clear()
            this.isWrong = true
            clearTimeout(this.isClickedTimer)
          } else {
            if (this.playerSequel.length == this.sequel.length) {
              clearTimeout(this.isClickedTimer)
              this.round++

              setTimeout(() => {
                this.playerSequel = []
                this.restart()
              }, 2000)
            }
          }
        }
      }
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
          this.playSound(this.sequel[this.currentIndex])
          this.currentShow = this.sequel[this.currentIndex]
          this.currentIndex++
        }

        setTimeout(() => {
          this.currentShow = null
        }, 300)
      }, 500)
    },
    playSound: function(id) {
      const sound = new Audio(require(`@/assets/audio/${Number(id) + 1}.mp3`))
      sound.play()
    },
    getNext: function() {
      let max = 3
      let min = 0
      return (Math.random() * (max - min) + min).toFixed(0)
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
        this.playSound(id)
        this.isClicked = true
        clearTimeout(this.isClickedTimer)
        console.log('click')
        this.playerSequel.push(id)
        this.checkClicked()
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
