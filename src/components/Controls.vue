<template>
   <div class="tempo-controls">
      <div class="tempo-controls__tempo">
         <button class="tempo-controls__tempo-btn"
            @click="slowDown">−</button>
         <div class="tempo-controls__tempo-value">{{ tempo }}</div>
         <button class="tempo-controls__tempo-btn"
            @click="speedUp">+</button>
      </div>
      <div class="tempo-controls__increment">
         <span aria-label="Increment by" title="Increment by">±</span>
         <label v-for="value in incrementValues">
            <input type="radio" v-model="increment" :value="value" />{{ value }}
         </label>
      </div>
   </div>
</template>

<script>
export default {
   data () {
      return {
         increment: 5,
         incrementValues: [1, 5, 10, 20],
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
   line-height: 2em;
   font-size: 1.3em;
}

.tempo-controls__tempo-value {
   font-weight: bold;
}

.tempo-controls__increment {
   margin-top: 1.5em;
}

.tempo-controls__tempo-btn {
   border: none;
   cursor: pointer;
}

</style>
