<template>
  <img :src="require(`@/assets/${state.image}.png`)" id="wakeup"  class="state"
  v-bind:class="{ active: isActive }">
</template>

<script>
export default {
  name: 'State',
  props: {
    state: Object
  },
  computed: {
    isActive: function() {
      return this.state.active
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
