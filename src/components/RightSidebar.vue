<!-- 
Komponent wyswietla: 
- podpowiedzi dla uzytkownika (Komponent Tips)
- dane na temat wybranej felgi
 -->

<template>
  <div class="sidebar">
    <div v-if="isRimSelected" class="rim-info">
      <h2 class="rim-info__title">{{ rimData.rimName }}</h2>
      <span class="rim-info__category">{{ rimData.category }}</span>
      <span class="rim-info__price">{{ rimData.price }} zł/szt </span>
      <span class="rim-info__set-price">Komplet {{ rimData.price*4 }} zł</span>
      <p class="rim-info__dsc">{{ rimData.desc }}</p>
      <button v-on:click="order" class="rim-info__button button">Zamów</button>
    </div>

    <right-sidebar-tips 
      v-else
      v-bind:appState="appState"
    />

  </div>
</template>

<script>
import RightSidebarTips from './RightSidebarTips.vue'
import STATES from '@/app/states';

export default {
  name: 'Sidebar',
  components: {
    RightSidebarTips
  },

  props: {
    rimData: Object,
    appState: String,
  },

  computed: {
    isRimSelected() {
      return this.appState === STATES.RIM_SELECTED;
    },
  },

  methods: {
    order () {
      alert(`Po kliknięciu następuje przekierowanie do strony sklepu dającego moliwość zakupu felgi o nazwie: ${this.rimData.rimName}.`)
    }
  }
}
</script>


<style scoped>

.sidebar {
  border-radius: 5px;
  background-color: rgb(241, 241, 241);
  width: 100%;
  height: 100%;
  padding: 20px;
  font-size: 14px;
}

.rim-info span {
  display: block;
}

.rim-info__title {
  font-size: 16px;
  margin: 0px;
  padding: 0px;
}

.rim-info__category {
  font-size: 12px;
}

.rim-info__price, .rim-info__set-price {
  text-align: right;
}

.rim-info__price {
  font-size: 16px;
  font-weight: bold;
}

.rim-info__button {
  border: none;
  margin: 0px auto;
  margin-top: 20px;
}

</style>
