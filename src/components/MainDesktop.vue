<!-- 

Komponent na podstawie biezącego stanu aplikacji udostępnia: 
- opcję wgrania zdjęcia
- główną funkcjonalność "rysowania" i manipulacji felgami na zdjęciu

-->

<!-- 
1. pomieszana składnia `@` i `v-on` - brak konsekwencji
-->

<template>
  <div class="desktop">
    <div v-if="showUploadRequest" class="uploader">
      <label for="image-input" class="button">
        <img src="../assets/upload.png">
        <span>Kliknij, żeby wgrać zdjęcie</span>
      </label>
      <input
        id="image-input"
        type="file"
        accept="image"
        @change="photoUploading"
      >
    </div>

    <div v-else 
      class="bg-picture" 
      v-bind:style="previewImageAsBg"
    >
      <drawing-board 
        v-bind:appState="appState"
        v-bind:rimPic="rimPic"
        @changeState="$emit('changeState', states.SELECT_RIM)"
        @resetState="$emit('changeState', states.UPLOAD_PHOTO)"
       />
    </div>

  </div>

</template>

<script>
import STATES from '@/app/states';
import DrawingBoard from './MainDesktop/DrawingBoard.vue';

export default {
  name: 'MainDesktop',

  model: {
    prop: 'appState',
    event: 'changeState',
  },

  components: {
    DrawingBoard
  },

  data() {
    return {
      states: STATES,
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
      this.$emit('changeState', this.states.DRAW_RIM_MOCKUPS);
      const image = e.target.files[0];
      this.previewImage = URL.createObjectURL(image);
    },
  },

  computed: {
    previewImageAsBg () {
      return `
      background-image: url("${this.previewImage}");
      background-position: center;
      background-repeat: no-repeat;
      background-size: cover;
      `
    },

    showUploadRequest() {
      return this.appState === STATES.UPLOAD_PHOTO;
    },
  },
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
