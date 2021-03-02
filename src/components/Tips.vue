<!-- Podpowiedzi wyświetlane na podstawie stanu aplikacji -->

<template>

  <div class="tips">

      <div v-if="this.appState == 'uploadPhoto'">
          <h1 class="tips__title">Wgraj zdjęcie</h1>
          <p>Wgraj zdjęcie swojego samochodu ustawionego prostopadle do obiektywu.</p>
          <img alt="tip" src="../assets/jak-zdjecie.png">
      </div>

      <div v-if="this.appState == 'drawRimMockups'">
          <h1 class="tips__title">Zaznacz na zdjęciu koła</h1>
          <p>Zaznacz średnicę kół.</p>
          <ol>
              <li>Kliknij jednokrotnie na początku lini przecięcia koła.</li>
              <li>Kliknij jednokrotnie na końcu lini przecięcia koła.</li>
          </ol>
          <img alt="instrukcja" src="../assets/instrukcja.png">
      </div>

      <div v-if="this.appState == 'selectRim'" class="tips__select-rim">
          <h1 class="tips__title">Wybierz felgę z listy</h1>
          <img alt="Arrow" src="../assets/arrow.png">
      </div>

  </div>

</template>

<script>
export default {
  name: 'Tips',

  props: {
    appState: String
  }

}
</script>


<style scoped>

.tips {
  display: flex;
  height: 100%;
}

.tips__title {
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

.tips__select-rim {
  align-self: flex-end;
  margin-bottom: 30px;
}

</style>
