<!-- 
Dynamicznie odbiera dane o wybranej feldze z app za pomocą props. 
 -->

<template>

  <div class="mockup" v-bind:style="changeColor">

    <div v-if="this.rimPosition.width == undefined" class="start-indicator"></div>
    <div v-if="this.rimPosition.width == undefined" class="line"></div>

    <button 
      v-if="(this.appState == 'selectRim' || this.appState == 'rimSelected') && !this.manipulator" 
      v-on:click="toggleManipulator" class="edit-button"
    >
    ✎
    </button>

    <div class="manipulator">
        <div v-if="this.manipulator">
          <button class="manipulator__button" v-on:click="moveRim('left')">◀</button>
          <button class="manipulator__button" v-on:click="moveRim('top')">▲</button>
          <button class="manipulator__button" v-on:click="moveRim('bottom')">▼</button>
          <button class="manipulator__button" v-on:click="moveRim('right')">▶</button>
          <button class="manipulator__button manipulator__button--zoom" v-on:click="moveRim('zoomIn')">+</button>
          <button class="manipulator__button manipulator__button--zoom" v-on:click="moveRim('zoomOut')">-</button>
          <button class="manipulator__button manipulator__button--done" v-on:click="toggleManipulator">✓</button>
        </div>
    </div>

  </div>

</template>

<script>
export default {
  name: 'DrawingBoardRimMockup',

  props: {
    rimPic: String,
    rimPosition: Object,
    tempWidth: Number,
    appState: String
  },

  data() {
    return {
      manipulator: false
    }
  },

  computed: {

    changeColor () {

      let x = this.rimPosition.x;
      let y = this.rimPosition.y;

      let width = 0;
      
      if (this.rimPosition.width == undefined) {
        width = this.tempWidth;
      } else {
        width = this.rimPosition.width; 
      }

      let bgpic = this.rimPic;

      return `
          top: ${y-(width/2)}px;
          left: ${x}px;
          width: ${width}px;
          height: ${width}px;
          border-radius: ${width/2}px;
          background-image: url("${bgpic}");
          `
    }

  },

  methods: {

    toggleManipulator () {
      this.manipulator = !this.manipulator;
    },

    moveRim(direction) {
      this.$emit('moveRim', this.rimPosition.id, direction)
    }

  }

}
</script>


<style scoped>

.mockup {
  position: absolute;
  background-color: rgb(48, 48, 48);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.start-indicator {
  width: 4px;
  height: 4px;
  border-radius: 2px;
  background-color: greenyellow;
  left: -2px;
  top: calc(50% - 2px);
  position: absolute;
}

.line {
  height: 2px;
  width: 100%;
  top: calc(50% - 1px);
  background-color:greenyellow;
  position: absolute;
}

.edit-button {
  top: -20px;
  right: -20px;
  position: absolute;
}

.manipulator {
  width: 115px;
  top: -70px;
  left: -40px;
  position: absolute;
  background-color: rgba(255, 255, 255, 0.774);
  border-radius: 5px;
}

.manipulator__button, .edit-button {
  width: 18px;
  height: 18px;
  border-radius: 9px;
  border: none;
  padding: 0px;
  margin: 5px;
  font-size: 12px;
  line-height: 12px;
  background-color: white;
}

.manipulator__button:hover, .edit-button:hover {
  background-color: lightgray;
}

.manipulator__button--done:hover {
  background-color: rgb(138, 230, 0);
}

.manipulator__button--zoom {
  font-size: 14px;
  line-height: 14px;
  font-weight: bold;
}

</style>
