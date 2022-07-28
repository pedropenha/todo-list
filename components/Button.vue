<template>
  <button :class="classButton === '' ? 'button-style' : classButton" @click.prevent="$emit('action')" :type="typeButton">
    <slot></slot>
    <span v-if="icon.length > 0" class="material-icons">{{ icon }}</span>
  </button>
</template>

<script>
export default{
  props:{
    classButton:{
      type: String,
      default: ''
    },
    icon:{
      type: String,
      default: ''
    },
    typeButton:{
      type: String,
      default: 'button'
    }
  }
}
</script>

<style scoped>
.button-style{
  background-color: transparent;
  border: none;
}
</style>
