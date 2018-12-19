<template>
  <div id="app">
    <Metronome
      @tick="showTick" />
  </div>
</template>

<script>
import Metronome from "./components/Metronome.vue"

const initialAnimationDuration = 250

export default {
   name: 'app',
   data () {
      return {
         animationDuration: null,
         currentDelay: null,
         tickDisplayMode: "toggle" // or "pulse"
      }
   },
   methods: {

      removeTickClass () {
         this.$el.removeEventListener("animationend", this.removeTickClass)
         this.$el.classList.remove("animationend", "tick")
      },

      showTick (delay) {
         if (this.tickDisplayMode === "pulse") {
            this.showTickPulse(delay)
         } else if (this.tickDisplayMode === "toggle") {
            this.showTickToggle()
         }
      },

      showTickToggle () {
         this.$el.classList.toggle("tick-on")
      },

      showTickPulse (delay) {
         this.$el.addEventListener("animationend", this.removeTickClass, false)

         if (delay !== this.currentDelay) {
            this.currentDelay = delay

            // If delay has changed, ensure animation duration is
            // always smaller than delay, to avoid overlapping animations
            if (delay < initialAnimationDuration) {
               this.animationDuration = delay;
            } else if (this.animationDuration !== initialAnimationDuration) {
               this.animationDuration = initialAnimationDuration
            }
         }

         this.$el.classList.add("tick")
      }

   },

   watch: {
      animationDuration: {
         immediate: true,
         handler: function (value, oldValue) {
            if (this.$el) {
               this.$el.style.animationDuration = (value / 1000) + "s"
            }
         }
      }
   },

   mounted () {
   },

   components: {
      Metronome
   },
}
</script>

<style>
#app {
   display: flex;
   justify-content: center;
   align-items: center;
   font-family: 'Avenir', Helvetica, Arial, sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   text-align: center;
   color: #2c3e50;
   height: 100%;
   padding: 1em;
}

@keyframes tick {
   from {
      background-color: #333;
      color: #eee;
   }

   to {
      background-color: initial;
      color: initial;
   }
}

#app.tick {
   animation-name: tick;
   animation-timing-function: cubic-bezier(0, .5, 0, .5);
}

#app.tick-on {
   background-color: #222;
   color: #eee;
}
</style>
