<template>
  <div :class="activeModal !== false ? 'modal is-active':'modal'">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">{{title}}</p>
        <button class="delete" aria-label="close" @click="$emit('closeModal')"></button>
      </header>
      <section class="modal-card-body">
        <slot></slot>
      </section>
      <footer class="modal-card-foot">
        <button class="button is-success" @click="$emit('save')">{{buttonSave}}</button>
        <button class="button" @click="$emit('cancel')">{{buttonCancel}}</button>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    title: {
      type: String,
      required: true
    },
    buttonSave:{
      type: String,
      default: 'Save'
    },
    buttonCancel:{
      type: String,
      default: 'Cancel'
    },
    activeModal:{
      type: Boolean,
      default: false
    }
  }
}
</script>
