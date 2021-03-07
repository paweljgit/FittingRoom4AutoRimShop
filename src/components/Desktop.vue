<!-- 

Komponent na podstawie biezącego stanu aplikacji udostępnia: 
- opcję wgrania zdjęcia
- główną funkcjonalność "rysowania" i manipulacji felgami na zdjęciu

-->


<template>

  <div class="desktop">

    <div v-if="this.appState == 'uploadPhoto'" class="uploader">

      <label for="image-input" class="button">
        <img src="../assets/upload.png">
        <span>Kliknij, żeby wgrać zdjęcie</span>
      </label>
      <input id="image-input" type="file" accept="image" v-on:change="photoUploading" >
      
    </div>

    <div v-if="this.appState != 'uploadPhoto'" 
      class="bg-picture" 
      v-bind:style="PreviewImageAsBg"
    >
      <DrawingBoard 
        v-bind:appState="this.appState"
        v-bind:rimPic="this.rimPic"
        @changeState="changeState('selectRim')"
        @resetState="changeState('uploadPhoto')"
       />

    </div>

  </div>

</template>

<script>

import DrawingBoard from './Desktop/DrawingBoard.vue';

export default {

  name: 'Desktop',

  components: {
    DrawingBoard
  },

  data() {
    return {
      previewImage: ''
    }
  },

  props: {
    rimData: Object,
    appState: String,
    rimPic: String
  },

  methods: {

    photoUploading(e) {
      this.$emit('changeState', 'drawRimMockups');
      const image = e.target.files[0];
      this.previewImage = URL.createObjectURL(image);
    },

    changeState(e) {
      this.$emit('changeState', e)
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

.bg-picture {
  border-radius: 5px 5px 0px 0px;
  border: solid 1px whitesmoke;
  width: 100%;
  height: 100%;
  background-color: rgb(241, 241, 241)
}


</style>
