<template>
   <div class="Metronome">
      <div class="visualization"></div>
      <tempo-controls v-bind:tempo="tempo" v-bind:running="isRunning"
         @change-tempo-by="changeTempoBy"
         @set-tempo="setTempo"
         @start="startTick"
         @stop="stop"
      />
      <tick-sound ref="audio" />
   </div>
</template>

<script>
import TempoControls from "./Controls.vue"
import TickSound from "./TickSound.vue"

const minimumTempo = 0;
const maximumTempo = 420;

export default {
   data () {
      return {
         tempo: 100,
         silent: true,
         ongoingTick: null,
         autostart: false,
         isRunning: false,
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
         this.isRunning = true

         let delay = (60 / this.tempo) * 1000
         if (this.ongoingTick) {
            clearTimeout(this.ongoingTick)
         }
         this.ongoingTick = this.tick(delay)
      },

      tick (delay) {
         if (this.isRunning) {
            if (delay) {
               console.info("Tick!", delay)
               this.$emit("tick", delay)
               this.$refs.audio.$emit("tick", delay)

               return setTimeout(() => {
                  this.ongoingTick = this.tick(delay)
               }, delay)
            } else {
               clearTimeout(this.ongoingTick)
               return null
            }
         }
      },

      stop () {
         if (this.ongoingTick) {
            clearTimeout(this.ongoingTick)
         }

         this.isRunning = false
      },

   },

   mounted () {
      // Attempt to prevent occasional concurrent timeouts possibly linked to hot-reloading
      if (this.ongoingTick) {
         clearTimeout(this.ongoingTick)
      }

      if (this.autostart) {
         this.startTick()
      }
   },

   components: {
      TempoControls,
      TickSound,
   },
}
</script>

<style>
.Metronome {
   border: .2em solid currentColor;
   padding: 1.5em;
}
</style>
