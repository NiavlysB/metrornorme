<template>
   <div class="Metronome">
      <div class="visualization">[{{ tempo }}]</div>
      <controls v-bind:tempo="tempo" v-on:change-tempo-by="changeTempoBy" />
   </div>
</template>

<script>
import Controls from "./Controls.vue"

export default {
   data () {
      return {
         tempo: 80
      }
   },
   components: {
      Controls
   },
   methods: {
      setTempo (newTempo) {
         this.tempo = newTempo
      },
      changeTempoBy (value) {
         this.tempo += value
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
