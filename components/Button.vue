<template>
  <button :class="classButton" @click.prevent="$emit('action')">
    <slot></slot>
    <span class="material-icons">{{ icon }}</span>
  </button>
</template>

<script>
export default{
  props:{
    classButton:{
      type: String,
      default: 'button is-info'
    },
    icon:{
      type: String,
      default: 'edit'
    }
  }
}
</script>

<style scoped>

</style>
