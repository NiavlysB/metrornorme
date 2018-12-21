<template>
   <div class="tempo-controls">
      <section class="tempo-controls__tempo">
         <button class="tempo-controls__tempo-btn"
            @click="slowDown">−</button>
         <div class="tempo-controls__tempo-value">{{ tempo }}</div>
         <button class="tempo-controls__tempo-btn"
            @click="speedUp">+</button>
      </section>
      <section class="tempo-controls__buttons">
         <button class="tempo-controls__button" @click="startStop">{{ running ? "⏸" : "▶" }}</button>
         <!-- <button class="tempo-controls__button">🔇</button> -->
      </section>
      <section class="tempo-controls__increment">
         <span aria-label="Increment by" title="Increment by">±</span>
         <label v-for="value in incrementValues" class="tempo-controls__increment-value"
            :data-checked="value == increment">
            <input type="radio" v-model="increment" :value="value" />
            {{ value }}
         </label>
      </section>
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
      running: {
         type: Boolean,
         required: true,
      }
   },
   methods: {
      speedUp () {
         this.$emit("change-tempo-by", this.increment, this.roundIncrement)
      },
      slowDown () {
         this.$emit("change-tempo-by", -this.increment, this.roundIncrement)
      },
      startStop () {
         if (this.running) {
            this.$emit("stop")
         } else {
            this.$emit("start")
         }
      },
      stop () {
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

.tempo-controls > section:not(:first-child) {
   margin-top: 1.5em;
}

.tempo-controls__tempo-btn {
   border: none;
   cursor: pointer;
}

.tempo-controls__button {
   background: transparent;
   font-size: 1em;
   border: none;
   color: currentColor;
   cursor: pointer;
}

.tempo-controls__increment-value {
   position: relative;
   display: inline-block;
   cursor: pointer;
   padding: .3em .6em;
   border: .1em solid transparent;
}

.tempo-controls__increment-value[data-checked] {
   border-color: currentColor transparent;
}

.tempo-controls__increment-value > input {
   position: absolute;
   appearance: none;
   -moz-appearance: none;
}
</style>
