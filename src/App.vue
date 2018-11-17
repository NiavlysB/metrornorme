<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <Metronome
      @tick="showTick()" />
  </div>
</template>

<script>
import Metronome from "./components/Metronome.vue"

export default {
   name: 'app',
   data () {
      return {
         title: 'Métronome'
      }
   },
   methods: {

      showTick () {
         this.$el.classList.toggle("tick")
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

#app.tick {
   background: black;
   color: white;
}

h1, h2 {
   font-weight: normal;
}

ul {
   list-style-type: none;
   padding: 0;
}

li {
   display: inline-block;
   margin: 0 10px;
}

a {
   color: #42b983;
}
</style>
