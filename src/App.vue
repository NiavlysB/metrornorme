<template>
  <div id="app">
    <Metronome
      @tick="showTick" />
  </div>
</template>

<script>
import Metronome from "./components/Metronome.vue"

export default {
   name: 'app',
   data () {
      return {}
   },
   methods: {

      removeTickClass () {
         this.$el.removeEventListener("animationend", this.removeTickClass)
         this.$el.classList.remove("animationend", "tick")
      },

      showTick () {
         this.$el.addEventListener("animationend", this.removeTickClass, false)
         this.$el.classList.add("tick")
      }

   },
   components: {
      Metronome
   },
}
</script>

<style>
#app {
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
   animation: tick .25s;
   animation-timing-function: cubic-bezier(0, .5, 0, .5);
}
</style>
