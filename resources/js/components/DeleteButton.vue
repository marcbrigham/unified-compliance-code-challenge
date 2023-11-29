<template>
    <button class="btn btn-danger" @click="deleteClicked">Delete</button>
  </template>

  <script>
  export default {
    methods: {
      deleteClicked() {
        this.$emit('delete-clicked');
      },
    },
  };
  </script>
