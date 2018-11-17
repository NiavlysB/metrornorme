<template>
   <div class="Metronome">
      <div class="visualization">[{{ tempo }}]</div>
      <tempo-controls v-bind:tempo="tempo" v-on:change-tempo-by="changeTempoBy" />
   </div>
</template>

<script>
import TempoControls from "./Controls.vue"

const minimumTempo = 0;
const maximumTempo = 420;

function setTempo(newTempo) {
   this.tempo = Math.min(
      Math.max(minimumTempo, newTempo),
      maximumTempo
   )
}

function changeTempoBy(increment, roundIncrement) {
   if (!roundIncrement) {
      this.setTempo(this.tempo + increment)
   } else {
      this.setTempo(increment * Math.floor(
         (this.tempo + increment) / increment
      ))
   }
}

export default {
   data () {
      return {
         tempo: 80,
         silent: true,
      }
   },
   components: {
      TempoControls
   },
   methods: {
      setTempo,
      changeTempoBy,
   }
}
</script>

<style>
.Metronome {
   border: .2em solid #333;
   padding: 1.5em;
}
</style>
