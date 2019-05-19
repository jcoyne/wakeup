<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <state
      v-for="state in states"
      v-bind:key="state.image"
      v-bind:active="state.active"
      v-bind:state="state"
      v-on:state-change="displayMessage"
    ></state>
    <!-- set progressbar -->
    <audio :src="`${publicPath}ding.mp3`" id="audio"></audio>
    <ul id="messages">
      <li v-for="item in messages">
        {{item}}
      </li>
    </ul>

    <div>
      <button @click="testButtonClicked">{{stepLabel}}</button>
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
      timeOffset: 0, // Set to 0 in production
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
    stepLabel: function() {
      if (this.inactive.length == 0)
        return 'Reset'

      return 'Test next step'
    },
    time() {
      return this.now.getHours() *100 + this.now.getMinutes()
    },
    inactive() {
      return this.states.filter(state => !state.active)
    }
  },
  methods: {
    displayMessage: function(event) {
      this.messages.unshift(`Time to ${event}!`)
      this.playSound()
    },
    playSound: function() {
      document.getElementById('audio').play()
    },
    testButtonClicked: function(event) {
      if (this.inactive.length == 0) {
        return this.resetSteps()
      }

      this.inactive[0].active = true
    },
    resetSteps: function(){
      this.messages = []
      this.states.forEach((state) => { state.active = false } )
    },
    isLaterInTheDay: function() {
      return this.time - this.states[0].time > 200
    },
    updateState: function() {
      this.inactive.forEach((state) => {
        let triggerTime = state.time + this.timeOffset
        if (triggerTime <= this.time && !this.isLaterInTheDay()) {
          state.active = true
        }
      })
    }
  },
  created () {
    this.sound = document.getElementById('audio')

    this.interval = setInterval(() => {
      this.now = new Date()
      this.updateState()
    }, 1000)
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
ul {
  list-style: none;
  font-size: 18pt;
}
</style>
