<!-- Dane dotyczące listy felg pobierane z pliku json. Dane klikniętej felgi emitowane do innych komponentów. -->

<template>

  <div class="rims-list">
    <rim-image
      v-for="{ rimName, id, active, photo } in rimsdb"
      :key="id"
      :name="rimName"
      :active="active"
      :photo="photo"
      @click="rimClicked(id)"
    />
  </div>

</template>

<script>
import rimsData from '../assets/rims-data.json';
import RimImage from './RimImage.vue';

export default {
  name: 'RimsList',
  components: { RimImage },

  data() {
    return {
      rimsdb: rimsData
    }
  },

  props: {
    appState: String
  },

  methods: {
    rimClicked(id) {
      const selectedRim = this.rimsdb[id];

      this.$emit('rimClicked', selectedRim);

      this.activateRim(id);
    },

    activateRim(activationId) {
      this.rimsdb = this.rimsdb.map(({ id, ...rest }) => ({
        ...rest,
        id,
        active: id === activationId,
      }));
    },
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


</style>
