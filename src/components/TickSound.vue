<template>
   <div class="audio-pool">
      <button @click="playTick">Tick</button>
   </div>
</template>

<script>
import tickSoundFile from "../assets/sounds/tick-1.wav"

export default {
   data () {
      return {
         tickSound: null
      }
   },
   methods: {

      playTick () {
         console.log("Tick event received!")
         if (this.tickSound !== null) {
            this.tickSound.play()
         }
      }

   },
   mounted () {
      this.tickSound = document.createElement("audio")
      this.tickSound.src = tickSoundFile

      this.$on("tick", this.playTick)
   }
}
</script>
