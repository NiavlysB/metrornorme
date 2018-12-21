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
         tickSoundBuffer: null,
         useRealSound: true,
         noiseFrequencies: []
      }
   },
   methods: {

      playTick () {
         console.log("Tick event received!")

         if (this.useRealSound && this.tickSoundBuffer instanceof AudioBuffer) {
            let tickSound = this.context.createBufferSource()
            tickSound.buffer = this.tickSoundBuffer
            tickSound.connect(this.context.destination)
            tickSound.start()
         } else {
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

      fetch(tickSoundFile)
         .then(response => response.arrayBuffer())
         .then(data => {
            console.log(data)
            this.context.decodeAudioData(data, buffer => {
               this.tickSoundBuffer = buffer
            })
         })

      this.$on("tick", this.playTick)
   }
}
</script>
