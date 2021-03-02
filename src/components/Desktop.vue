<!-- 

Komponent na podstawie biezącego stanu aplikacji udostępnia: 
- Opcję wgrania zdjęcia.
- Mozliwość zaznaczania na zdjęciu miejsc gdzie znajdują się felgi. Na podstawie 4 kliknięć wyznaczane są pozycje i wymiary kół.
- Mozliwość zrestartowania stanu aplikacji i ponownego wgrania zdjęcia.

-->


<template>

  <div class="desktop">

    <div v-if="this.appState == 'uploadPhoto'" class="uploader">

      <label for="image-input" class="button">
        <img src="../assets/upload.png">
        <span>Kliknij, zeby wgrać zdjęcie</span>
      </label>
      <input id="image-input" type="file" accept="image" v-on:change="photoUploading" >
      
    </div>

    <div 
      v-if="this.appState != 'uploadPhoto'" 
      class="drawing-board" 
      v-bind:style="PreviewImageAsBg" 
      v-on:click="firstFourClicks" 
    >

      <RimMockup  
        v-if="this.rimMockups.firstRim.secondClickPosition[0] != undefined" 
        class="rim-mockup" 
        v-bind:style="firstRimMockupPosition" 
        v-bind:firstClickPosition="this.rimMockups.firstRim.firstClickPosition" 
        v-bind:secondClickPosition="this.rimMockups.firstRim.secondClickPosition" 
      />

      <RimMockup 
        v-if="this.rimMockups.secondRim.secondClickPosition[0] != undefined" 
        class="rim-mockup" 
        v-bind:style="secondRimMockupPosition"
        v-bind:firstClickPosition="this.rimMockups.secondRim.firstClickPosition" 
        v-bind:secondClickPosition="this.rimMockups.secondRim.secondClickPosition"
      />

      <button class="drawing-board__button button" 
        v-if="this.rimMockups.secondRim.secondClickPosition[1] != undefined" 
        v-on:click="cleaner"
      >
        Zmień zdjęcie
      </button>

    </div>

  </div>

</template>

<script>

import RimMockup from './Desktop/RimMockup.vue'

export default {

  name: 'Desktop',

  components: {
    RimMockup
  },

  data() {
    return {
      previewImage: '',
      clickCounter: 0,
      rimMockups: {
        firstRim: {
          firstClickPosition: [],
          secondClickPosition: [],
        },
        secondRim: {
          firstClickPosition: [],
          secondClickPosition: [],
        }
      }
    }
  },

  props: {
    rimData: Object,
    appState: String
  },

  methods: {

    firstFourClicks (event) {

    this.clickCounter++;

      if (this.clickCounter == 1) {

        this.rimMockups.firstRim.firstClickPosition.push(event.offsetX);
        this.rimMockups.firstRim.firstClickPosition.push(event.offsetY);

      } else if (this.clickCounter == 2 ) {

        this.rimMockups.firstRim.secondClickPosition.push(event.offsetX);
        this.rimMockups.firstRim.secondClickPosition.push(event.offsetY);

      } else if (this.clickCounter == 3 ) {

        this.rimMockups.secondRim.firstClickPosition.push(event.offsetX);
        this.rimMockups.secondRim.firstClickPosition.push(event.offsetY);

      } else if (this.clickCounter == 4 ) {

        this.rimMockups.secondRim.secondClickPosition.push(event.offsetX);
        this.rimMockups.secondRim.secondClickPosition.push(event.offsetY);

        this.$emit('changeState', 'selectRim')

      }

    },

    cleaner() {
      this.rimMockups.firstRim.firstClickPosition.splice(0,2);
      this.rimMockups.firstRim.secondClickPosition.splice(0,2);
      this.rimMockups.secondRim.firstClickPosition.splice(0,2);
      this.rimMockups.secondRim.secondClickPosition.splice(0,2);
      this.clickCounter = 0;
      this.$emit('changeState', 'uploadPhoto')
    },

    photoUploading(e) {
      this.$emit('changeState', 'drawRimMockups');
      const image = e.target.files[0];
      this.previewImage = URL.createObjectURL(image);
    }

  },

  computed: {

    PreviewImageAsBg () {
    return `
    background-image: url("${this.previewImage}");
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    `
    },

    firstRimMockupPosition () {

      var xClick = this.rimMockups.firstRim.firstClickPosition[0];
      var yClick = this.rimMockups.firstRim.firstClickPosition[1];
      var width = this.rimMockups.firstRim.secondClickPosition[0] - xClick;
      var topPosition = (yClick-(width/2))
      var height = width;
      var radius = width/2;
      var bgpic = this.rimData.photo;

      return `
      left: ${xClick}px; 
      top: ${topPosition}px; 
      width: ${width}px; 
      height: ${height}px; 
      border-radius: ${radius}px;
      z-index: 1;
      background-image: url("${bgpic}");
      `
    },

    secondRimMockupPosition () {
      
      var xClick = this.rimMockups.secondRim.firstClickPosition[0];
      var yClick = this.rimMockups.secondRim.firstClickPosition[1];
      var width = this.rimMockups.secondRim.secondClickPosition[0] - xClick;
      var topPosition = (yClick-(width/2))
      var height = width;
      var radius = width/2;
      var bgpic = this.rimData.photo;

      return `
      left: ${xClick}px; 
      top: ${topPosition}px; 
      width: ${width}px; 
      height: ${height}px; 
      border-radius: ${radius}px;
      z-index: 2;
      background-image: url("${bgpic}");
      `
    }
  }

}

</script>

<style scoped>

.desktop {
  background-color: rgb(241, 241, 241);
  height: calc(100% - 100px);
  border-radius: 5px 5px 0px 0px;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.uploader {
  display: flex;
  height: max-content;
}

.uploader img {
  margin-right: 10px;
  width: 20px;
}

.uploader input[type="file"] {
  display: none;
}

.drawing-board {
  border-radius: 5px 5px 0px 0px;
  border: solid 1px whitesmoke;
  width: 100%;
  height: 100%;
  background-color: rgb(241, 241, 241)
}

.drawing-board  .drawing-board__button {
  font-size: 10px;
  border: none;
  padding: 7px 10px;
  margin: 10px;
  position: absolute;
  right: 0px;
  opacity: 0.5;
}

.rim-mockup {
  position: absolute;
  background-color: rgb(48, 48, 48);
  color: white;
  text-align: center;
  font-size: 8px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

</style>
