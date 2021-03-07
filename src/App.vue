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

<template>

  <div id="app">

    <div class="main-panel">

      <Desktop 
        v-on:changeState="changeState" 
        v-bind:rimPic="this.rimData.photo" 
        v-bind:appState="this.appState"
      />

      <RimsList 
        v-on:rimClicked="rimClicked" 
        v-bind:appState="this.appState"
      />

    </div>

    <div class="sidebar">

      <Sidebar 
        v-bind:rimData="this.rimData" 
        v-bind:appState="this.appState" 
      />

    </div>

  </div>

</template>

<script>

import Desktop from './components/Desktop.vue'
import RimsList from './components/RimsList.vue'
import Sidebar from './components/Sidebar.vue'

export default {
  name: 'App',
  components: {
    Desktop, RimsList, Sidebar
  },
  
  data() {
    return {
      rimData: {},
      appState: 'uploadPhoto'
    }

  },

  methods: {
    
    rimClicked(e) {
      if (this.appState == 'selectRim' || this.appState == 'rimSelected') {
        this.rimData = e,
        this.appState = 'rimSelected'
      } else {
        alert('Najpierw wgraj zdjęcie samochodu i zaznacz miejsca gdzie są felgi.')
      }
    },

    changeState(e) {
      this.appState = e
    }

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
