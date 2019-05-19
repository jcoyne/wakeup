<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <state
      v-for="state in states"
      v-bind:key="state.image"
      v-bind:state="state"
    ></state>
    <!-- set progressbar -->
    <audio :src="`${publicPath}ding.mp3`" id="audio"></audio>
    <ul id="messages">
      <li v-for="item in messages">
        {{item}}
      </li>
    </ul>

    <div>
      <button @click="step">Step {{state}}</button>
    </div>
  </div>
</template>

<script>
import State from './State.vue';

let interval = null

export default {
  name: 'HelloWorld',
  components: {State},
  props: {
    msg: String
  },
  data: function() {
    return {
      now: new Date(),
      state: 0,
      max: 4,
      offset: 0, // Set to 0 in production
      messages: [],
      publicPath: process.env.BASE_URL,
      states: [
        { image: 'wakeup', time: 550, active: false, label: 'wake up' },
        { image: 'toothbrushing', time: 610, active: false, label: 'brush teeth' },
        { image: 'breakfast', time: 620, active: false, label: 'eat breakfast' },
        { image: 'hairbrushing', time: 630, active: false, label: 'brush hair' },
        { image: 'bus', time: 645, active: false, label: 'go to the bus'}
      ]
    }
  },
  computed: {
    time() {
      return this.now.getHours() *100 + this.now.getMinutes()
    }
  },
  methods: {
    step: function(event) {
      document.getElementById('audio').play();
      if (this.state < this.states.length) {
        this.states[this.state].active = true
        this.state += 1
      } else {
        this.state = 0
        this.messages = []
        this.states.forEach((state) => { state.active = false } )
      }
    },
    isLaterInTheDay: function() {
      return this.time - this.states[0].time > 200
    },
    updateState: function() {
      var inactive = this.states.filter(state => !state.active)
      var sound = document.getElementById('audio')

      inactive.forEach((state) => {
        let triggerTime = state.time + this.offset
        if (triggerTime <= this.time && !this.isLaterInTheDay()) {
          state.active = true
          console.log(`${state.image} is now active`)
          this.messages.unshift(`Time to ${state.label}!`)
          sound.play()
        }
      })
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

ul {
  list-style: none;
  font-size: 18pt;
}
</style>
