<template>
   <div class="Metronome">
      <div class="visualization">[{{ tempo }}]</div>
      <tempo-controls v-bind:tempo="tempo" v-on:change-tempo-by="changeTempoBy" />
   </div>
</template>

<script>
import TempoControls from "./Controls.vue"

export default {
   data () {
      return {
         tempo: 80
      }
   },
   components: {
      TempoControls
   },
   methods: {
      setTempo (newTempo) {
         this.tempo = newTempo
      },

      changeTempoBy (increment, roundIncrement) {
         if (!roundIncrement) {
            this.tempo += increment
         } else {
            this.tempo = increment * Math.floor(
               (this.tempo + increment) / increment
            )
         }
      }
   }
}
</script>

<style>
.Metronome {
   border: .2em solid #333;
   padding: 1.5em;
}
</style>
