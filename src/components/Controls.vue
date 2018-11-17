<template>
   <div class="tempo-controls">
      <div class="tempo-controls__tempo">
         <button @click="slowDown()">−</button>
         <div class="tempo">{{ tempo }}</div>
         <button @click="speedUp()">+</button>
      </div>
      <div class="tempo-controls__increment">
         <span aria-label="Incrémenter de" title="Incrémenter de">±</span>
         <label v-for="value in incrementValues">
            <input type="radio" v-model="increment" :value="value" />{{ value }}
         </label>
         <label>
            <input type="checkbox" v-model="roundIncrement"
            />Arrondir
         </label>
      </div>
   </div>
</template>

<script>
export default {
   data () {
      return {
         increment: 5,
         incrementValues: [1, 5, 10],
         roundIncrement: true,
      }
   },
   props: {
      tempo: {
         type: Number,
         required: true
      },
   },
   methods: {
      speedUp () {
         this.$emit("change-tempo-by", this.increment, this.roundIncrement)
      },
      slowDown () {
         this.$emit("change-tempo-by", -this.increment, this.roundIncrement)
      },
   }
}
</script>

<style>
.tempo-controls__tempo {
   display: flex;
   width: 100%;
}

.tempo-controls__tempo > * {
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
