<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- set progressbar -->
    <img src="../assets/wakeup.png" id="wakeup" class="state">
    <img src="../assets/toothbrushing.png" id="toothbrushing" class="state">
    <img src="../assets/breakfast.png" id="breakfast" class="state">
    <img src="../assets/hairbrushing.png" id="hairbrushing" class="state">
    <img src="../assets/bus.png" id="bus" class="state">

    <div>
      <button @click="step">Step {{state}}</button>
      <input>{{time}}</input>
    </div>
  </div>
</template>

<script>
let interval = null

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: function() {
    return {
      now: new Date(),
      state: 0,
      max: 4,
      offset: 500, // Set to 0 in production
      states: {
        'wakeup': 550,
        'toothbrushing': 610,
        'breakfast': 620,
        'hairbrushing': 630,
        'bus': 645,
      }
    }
  },
  computed: {
    time() {
      return this.now.getHours() *100 + this.now.getMinutes()
    }
  },
  methods: {
    step: function(event) {
      if (this.state < this.max) {
        this.state += 1
      }
    },
    updateState: function() {
      for (var value in this.states) {
        let triggerTime = this.states[value] + this.offset
        if (triggerTime <= this.time) {
          console.log(`${value} is now active`)
          document.getElementById(value).className = 'state active'
        }
      }
    }
  },
  created () {
    this.interval = setInterval(() => {
      this.now = new Date()
      this.updateState()
    }, 1000)
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

h3 {
  margin: 40px 0 0;
}
</style>
