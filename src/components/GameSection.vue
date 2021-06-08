<template>
  <div :class="['section', color, {active: active}]">
    {{ activeInput }}
  </div>
</template>
<script>
export default {
  name: 'GameSection',
  props: {
    color: {
      type: String,
      required: true,
      default: 'gray'
    },
    activeInput: {
      type: Boolean
    }
  },
  data() {
    return {
      active: false
    }
  },
  mounted() {
    console.log('section active mounted')
    this.active = this.activeInput
    console.log(this.active)
  },
  updated() {
    console.log('section active updated')
    this.active = this.activeInput
    console.log(this.active)
    setTimeout(() => {
      this.active = false
    }, 200)
  }
  /*watch: {
    active: function(val) {
      setTimeout(() => {
        this.active = false
      }, 1000)
    }
  } */
}
</script>
