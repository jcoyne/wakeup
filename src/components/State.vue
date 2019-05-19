<template>
  <img :src="require(`@/assets/${state.image}.png`)" id="wakeup"  class="state"
  v-bind:class="{ active: isActive }">
</template>

<script>
export default {
  name: 'State',
  props: {
    state: Object,
    active: Boolean,
  },
  computed: {
    isActive: function() {
      return this.state.active
    }
  },
  watch: {
    active: function(newVal, oldVal) {
      if (!oldVal && newVal) {
        this.$emit('state-change', this.state.label)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.state {
  height: 200px;
	filter: grayscale(100%) contrast(50%);
}

.state.active {
  filter: none;
}
</style>
