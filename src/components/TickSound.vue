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
         context: null,
         tickSound: null,
         useRealSound: false,
         noiseFrequencies: []
      }
   },
   methods: {

      playTick () {
         console.log("Tick event received!")
         if (this.useRealSound && this.tickSound !== null) {
            this.tickSound.play()
         }

         if (this.useRealSound === false) {
            this.noiseFrequencies.forEach(frequency => {
               let oscillator = this.context.createOscillator()
               let gainNode = this.context.createGain()
               let duration = .2
               let now = this.context.currentTime

               oscillator.frequency.value = frequency
               gainNode.gain.setValueAtTime(gainNode.gain.value, now + (duration * .1))
               gainNode.gain.linearRampToValueAtTime(0.0, now + (duration * .9))
               oscillator.connect(gainNode)
               gainNode.connect(this.context.destination)

               oscillator.start(now)
               oscillator.stop(now + duration)
            })

         }
      }

   },
   mounted () {
      this.context = new AudioContext()

      this.noiseFrequencies = (new Array(42).fill(undefined)).map(
         _ => Math.floor(Math.random() * 4200) + 42
      )

      console.log(this.noiseFrequencies)

      this.tickSound = document.createElement("audio")
      this.tickSound.src = tickSoundFile

      this.$on("tick", this.playTick)
   }
}
</script>
