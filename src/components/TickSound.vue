<template>
   <div class="audio-pool">
      <button @click="playTick">Tick</button>
      <!--<audio ref="players" v-for="player in players" ></audio>-->
      <audio ref="player" type="audio/wav" preload="auto" src="./src/assets/sounds/tick-1.wav"></audio>
   </div>
</template>

<script>
// import tickSoundFile from "../assets/sounds/tick-1.ogg"

export default {
   data () {
      return {
         // tickSound: tickSoundFile
      }
   },
   methods: {

      playTick () {
         console.log("Tick event received!")
         this.$refs.player.play()
      }

   },
   mounted () {
      this.$on("tick", this.playTick)
   }
}
</script>
