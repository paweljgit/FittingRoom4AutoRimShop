<!-- Podpowiedzi wyświetlane na podstawie stanu aplikacji -->

<template>
  <div class="tips">
    <component :is="selectedComponent" class="tips__title" />
  </div>
</template>

<script>
import STATES from '@/app/states';
import DrawRimMockups from './tips/DrawRimMockups.vue';
import SelectRims from './tips/SelectRims.vue';
import UploadPhoto from './tips/UploadPhoto.vue';

export default {
  name: 'Tips',

  props: {
    appState: String
  },

  computed: {
    selectedComponent() {
      if (this.appState === STATES.UPLOAD_PHOTO) {
        return UploadPhoto;
      }

      if (this.appState === STATES.DRAW_RIM_MOCKUPS) {
        return DrawRimMockups;
      }

      return SelectRims;
    }
  }

}
</script>


<style scoped>

.tips {
  display: flex;
  height: 100%;
}

.tips__title ::v-deep h1 {
  font-size: 16px;
  margin: 0px;
  padding: 0px;
}

.tips img {
  margin: 0 auto;
  display: flex;
  margin-top: 40px;
}

.tips ol {
  padding-left: 20px;
}

</style>
