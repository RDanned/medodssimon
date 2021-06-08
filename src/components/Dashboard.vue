<template>
  <div class="dashboard">
    <button @click="$emit('start')">Start</button>
    <p>Round: {{ round }}</p>
  </div>
</template>
<script>
export default {
  name: 'Dashboard',
  props: {
    round: {
      type: Number
    }
  }
}
</script>
