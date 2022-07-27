<template>
  <div class="card-container">
    <div class="p-6 is-flex">
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
  cursor: pointer;
}

.card-container:hover{
  box-shadow: rgba(0, 0, 0, 0.24) 0 3px 8px;
}
</style>
