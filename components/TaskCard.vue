<template>
  <div class="card-container">
    <div class="p-6 is-flex is-flex-wrap-wrap">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
}
</script>

<style scoped>
.card-container{
  width: auto;
  border: 2px solid #e9e9e9;
  transition: all .5s ease;
}
</style>
