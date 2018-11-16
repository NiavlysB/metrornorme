<template>
   <div class="controls">
      <div class="tempo-controls">
         <button @click="slowDown()">−</button>
         <div class="tempo">{{ tempo }}</div>
         <button @click="speedUp()">+</button>
      </div>
      <div class="increment-controls">
         <span>±</span>
         <label v-for="value in possibleIncrements">
            <input type="radio" v-model="increment" :value="value" />{{ value }}
         </label>
      </div>
   </div>
</template>

<script>
export default {
   data () {
      return {
         increment: 1,
         possibleIncrements: [1, 5, 10],
      }
   },
   props: {
      tempo: Number
   },
   methods: {
      speedUp () {
         this.$emit("change-tempo-by", this.increment)
      },
      slowDown () {
         this.$emit("change-tempo-by", -this.increment)
      },
   }
}
</script>

<style>
.tempo-controls {
   display: flex;
   width: 100%;
}

.tempo-controls > * {
   background: transparent;
   color: currentColor;
   font-size: inherit;
   padding: .3em 1em;
   flex: 1;
}

button {
   cursor: pointer;
   border: 1px solid #aaa;
}
</style>
