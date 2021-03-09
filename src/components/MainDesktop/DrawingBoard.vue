<!-- 

Na podstawie kolejnych kliknięć wyznaczane są pozycje i wymiary kół.
- pierwsze kliknięcie wyznacza pozycje
- drugie kliknięcie wyznacza szerokość

Mozliwość zrestartowania stanu aplikacji i ponownego wgrania zdjęcia.

-->

<template>

  <div class="drawing-board" v-on:click="createRims(2)" v-on:mousemove="calcWidth">

    <div v-for="rim in rimPositions" v-bind:key="rim.id">
      <drawing-board-rim-mockup
        v-bind:rimPosition="rim" 
        v-bind:tempWidth="tempWidth"
        v-bind:appState="appState"
        v-bind:rimPic="rimPic"
        v-on:moveRim="moveRim"
      />
    </div>

    <button 
      class="drawing-board__button button"
      v-if="this.appState == 'selectRim' || this.appState == 'rimSelected'" 
      v-on:click="reset"
    >
    Reset
    </button>

  </div>

</template>

<script>

import DrawingBoardRimMockup from './DrawingBoardRimMockup.vue';

export default {
  name: 'DrawingBoard',

  components: {
    DrawingBoardRimMockup
  },

  data() {
    return {
      rimsCounter: 0,
      rimPositions: [ ],
      cursor: [ ],
      tempWidth: 0,
    }
  },

  props: {
    appState: String,
    rimPic: String,
  },

  methods: {

    createRims (maxQuantity) {    

      if (this.rimsCounter < maxQuantity) {

        this.rimsCounter =  this.rimsCounter + 0.5;

        if (!Number.isInteger(this.rimsCounter)) { 

          this.rimPositions.push({
            x: this.cursor[0], 
            y: this.cursor[1]});

        } else {

          this.rimPositions[this.rimPositions.length-1].width = this.tempWidth;
          this.rimPositions[this.rimPositions.length-1].id = this.rimsCounter-1

          if (this.rimPositions.length == maxQuantity) {

            this.$emit('changeState', 'selectRim');

          }

        }

      }

    },

    calcWidth (e) {

      this.cursor[0] = e.offsetX;
      this.cursor[1] = e.offsetY; 

      if (this.rimsCounter && !Number.isInteger(this.rimsCounter)) {

        let cursorX = this.cursor[0];
        let rimStartX = this.rimPositions[this.rimPositions.length-1].x;
        
        this.tempWidth = cursorX - rimStartX;

      } else {

        this.tempWidth = 0;

      }

    },

    reset () {
      this.$emit('resetState', 'uploadPhoto');
    },

    moveRim (e, direction) {

      if (direction == 'left') {
        this.rimPositions[e].x--
      } else if (direction == 'right') {
        this.rimPositions[e].x++
      } else if (direction == 'top') {
        this.rimPositions[e].y--
      } else if (direction == 'bottom') {
        this.rimPositions[e].y++
      } else if (direction == 'zoomIn') {
        this.rimPositions[e].x--;
        this.rimPositions[e].width++;
        this.rimPositions[e].width++
      } else if (direction == 'zoomOut') {
        this.rimPositions[e].x++;
        this.rimPositions[e].width--;
        this.rimPositions[e].width--
      }

    }

  }

}
</script>


<style scoped>

.drawing-board {
  width: 100%;
  height: 100%;
}

.drawing-board__button {
  font-size: 10px;
  border: none;
  padding: 7px 10px;
  margin: 10px;
  position: absolute;
  right: 0px;
  opacity: 0.5;
}

</style>
