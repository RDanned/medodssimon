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
  methods: {
    click: function() {
      this.$emit('click', this.id)
    }
  }
}
</script>
