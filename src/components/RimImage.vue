<template>
   <div 
      v-bind:class="{active : active === true}" 
      class="rim" 
      v-on:click="$emit('click')" 
    >
      <img v-bind:src="photo" class="rim__photo" loading="lazy" /> 
      <span class="rim__name">{{ name }}</span>

    </div>
</template>

<script>
export default {
  name: 'RimImage',

  props: {
    active: {
      type: Boolean,
      default: false,
    },

    photo: {
      type: String,
      required: true,
    },

    name: {
      type: String,
      required: true,
    }
  }
}
</script>

<style scoped>

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