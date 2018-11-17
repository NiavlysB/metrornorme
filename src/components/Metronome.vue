<template>
   <div class="Metronome">
      <div class="visualization">[{{ tempo }}]</div>
      <tempo-controls v-bind:tempo="tempo"
         @change-tempo-by="changeTempoBy"
         @set-tempo="setTempo"
      />
   </div>
</template>

<script>
import TempoControls from "./Controls.vue"

const minimumTempo = 0;
const maximumTempo = 420;

export default {
   data () {
      return {
         tempo: 80,
         silent: true,
         ongoingTick: null,
      }
   },
   methods: {

      setTempo (newTempo) {
         this.tempo = Math.min(
            Math.max(minimumTempo, newTempo), // accept no less than minimum tempo
            maximumTempo
         ) // and no more than maximumTempo

         this.startTick()
      },

      changeTempoBy (increment, roundIncrement) {
         if (!roundIncrement) {
            this.setTempo(this.tempo + increment)
         } else {
            this.setTempo(increment * Math.floor(
               (this.tempo + increment) / increment
            ))
         }
      },

      startTick () {
         let delay = (60 / this.tempo) * 1000
         if (this.ongoingTick) {
            clearTimeout(this.ongoingTick)
         }
         this.ongoingTick = this.tick(delay)
      },

      tick (delay) {
         if (delay) {
            console.info("Tick!", delay)
            this.$emit("tick")

            return setTimeout(() => {
               this.ongoingTick = this.tick(delay)
            }, delay)
         } else {
            console.error("No delay specified")
            clearTimeout(this.ongoingTick)
         }
      },

   },
   mounted () {
      this.startTick()
   },
   components: {
      TempoControls
   },
}
</script>

<style>
.Metronome {
   border: .2em solid #333;
   padding: 1.5em;
}
</style>
