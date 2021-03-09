<!-- 

OPIS KOMPONENTOW: 
  1. Komponent Desktop to główny komponent pozwalający na "przymiarki" felg do samochodu. 
  2. Komponent RimsList prezentuje listę dostępnych felg.
  3. Komponent Sidebar prezentuje podpowiedzi dla uzytkownika oraz dane dotyczące wybranej felgi. 

OPIS DANYCH:
  appState - Zawiera obecny stan aplikacji
  rimData - Zawiera dane dotyczące wybranej przez uzytkownika felgi

DODATKOWE INFO:
  Świadomie nie stosowałem vue shorthands dla utrwalenia syntax.

-->

<!--
1. this nie jest potrzben w template
-->

<template>

  <div id="app">

    <div class="main-panel">

      <main-desktop  
        v-bind:rimPic="rimData.photo" 
        v-model="appState"
      />

      <rims-list 
        v-bind:appState="appState"
        v-on:rimClicked="rimClicked" 
      />

    </div>

    <div class="sidebar">

      <right-sidebar 
        v-bind:rimData="rimData" 
        v-bind:appState="appState" 
      />

    </div>

  </div>

</template>

<script>
import STATES from '@/app/states';
import MainDesktop from './components/MainDesktop.vue'
import RimsList from './components/RimsList.vue'
import RightSidebar from './components/RightSidebar.vue'

export default {
  name: 'App',
  components: {
    MainDesktop, RimsList, RightSidebar
  },
  
  data() {
    return {
      rimData: {},
      appState: 'uploadPhoto'
    }
  },

  methods: {
    rimClicked(e) {
      if (this.appState === STATES.SELECT_RIM || this.appState == STATES.RIM_SELECTED) {
        this.rimData = e,
        this.appState = STATES.RIM_SELECTED;
      } else {
        alert('Najpierw wgraj zdjęcie samochodu i zaznacz miejsca gdzie są felgi.')
      }
    },
  }
}
</script>

<style>

* {
  box-sizing: border-box;
}

#app {
  width: 640px;
  height: 360px;
  padding: 5px;
  margin: 50px auto;
  background-color: white;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  box-sizing: border-box;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 14x;
  color: #2c3e50;
}

.main-panel {  
  height: 100%;
  flex-basis: calc(100% - 225px - 5px);
  flex-direction: column;
}

.sidebar {
  flex-basis: 225px;
}

.button {
  display: flex;
  padding: 10px 15px;
  background-color: #fff;
  cursor: pointer;
  border-radius: 5px;
  align-items: center;
  font-size: 14px;
  font-weight: 600;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px, rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
}

.button:hover {
  background-color: rgb(243, 243, 243);
}

:active {
  outline: 0;
}

</style>
