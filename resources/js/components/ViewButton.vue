<template>
    <button class="btn btn-primary" @click="viewClicked">View</button>
  </template>

  <script>
  export default {
    methods: {
      viewClicked() {
        this.$emit('view-clicked');
      },
    },
  };
  </script>
