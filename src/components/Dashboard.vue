<template>
  <div class="dashboard">
    <button v-if="showBtn" class="btn-start" @click="$emit('start')">
      Start
    </button>
    <p>Round: {{ round }}</p>
    <div>
      <p>Choose your complexility</p>
      <ul>
        <li>
          <input
            type="radio"
            name="severity"
            id="sev0"
            value="1500"
            checked
            @change="chooseSeverity"
          />
          <label for="sev0">easy</label>
        </li>
        <li>
          <input
            type="radio"
            name="severity"
            id="sev1"
            value="1000"
            @change="chooseSeverity"
          />
          <label for="sev1">medium</label>
        </li>
        <li>
          <input
            type="radio"
            name="severity"
            id="sev2"
            value="400"
            @change="chooseSeverity"
          />
          <label for="sev2">hard</label>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Dashboard',
  props: {
    round: {
      type: Number
    },
    showBtn: {
      type: Boolean
    }
  },
  methods: {
    chooseSeverity: function(e) {
      this.$emit('chooseSeverity', e.target.value)
    }
  }
}
</script>
