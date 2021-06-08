<template>
  <div :class="['section', color, {active: activeInput}]" @click="click"></div>
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
    },
    id: {
      type: Number
    }
  },
  data() {
    return {
      active: false
    }
  },
  mounted() {
    //console.log('section active mounted')
    this.active = this.activeInput
    //console.log(this.active)
  },
  updated() {
    //console.log('section active updated')
    this.active = this.activeInput
    //console.log(this.active)
    setTimeout(() => {
      this.active = false
    }, 200)
  },
  methods: {
    click: function() {
      this.$emit('click', this.id)
    }
  }
}
</script>
