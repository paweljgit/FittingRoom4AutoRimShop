<!-- Dane dotyczące listy felg pobierane z pliku json. Dane klikniętej felgi emitowane do innych komponentów. -->

<template>

  <div class="rims-list">

    <div 
      v-for="rim in rimsdb" 
      class="rim" 
      v-bind:key="rim.id" 
      v-on:click="rimClicked(rim)" 
      v-bind:class="{active : rim.active === true}" 
    >

      <img v-bind:src="rim.photo" class="rim__photo" loading="lazy" /> 
      <span class="rim__name">{{ rim.rimName }}</span>

    </div>

  </div>

</template>

<script>
import rimsData from '../assets/rims-data.json';

export default {
  name: 'RimsList',

  data() {
    return {
      rimsdb: rimsData
    }

  },

  props: {
  appState: String
  },

  methods: {
    
    rimClicked(e) {
      this.$emit('rimClicked', e);

      if(this.appState == 'selectRim' || this.appState == 'rimSelected') {
      this.rimsdb.forEach(element => {
        element.active = false
      });
      e.active = true
      }

    }

  }

}
</script>


<style scoped>

.rims-list {
  background-color: rgb(241, 241, 241);
  border-top: 1px solid white;
  height: 100px;
  width: 100%;
  border-radius: 0px 0px 5px 5px;
  padding: 10px;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  overflow-x: auto;
  overflow-y: hidden;
}

.rims-list::-webkit-scrollbar {
  display: none;
}

.rim {
  height: 80px;
  width: 80px;
  border-radius: 5px;
  background-color: white;
  margin-right: 10px;
  text-align: center;
  display: flex;
  flex-direction: column;
}

.active {
  border: 1px solid lightgray;
}

.rim__photo {
  height: 60px;
  width: 60px;
  display: flex;
  margin: 0 auto;
  padding: 5px;
}

.rim__name {
  font-size: 10px;
  line-height: 15px;
  height: 15px;
  overflow: hidden;
}

</style>
