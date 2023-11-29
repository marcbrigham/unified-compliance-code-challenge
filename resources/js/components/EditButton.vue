<template>
    <button class="btn btn-primary" @click="editClicked">Edit</button>
  </template>

  <script>
  export default {
    methods: {
      editClicked() {
        this.$emit('edit-clicked');
      },
    },
  };
  </script>
