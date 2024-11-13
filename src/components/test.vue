<template>
    <div>
      <h1>{{ inputText }}</h1>
      <input type="text" :placeholder="placeholder" v-model="inputText">
    </div>
  </template>
  
  <script setup>
  import { ref } from "vue";
  const placeholder = ref( "placeholder text" );
  const inputText = ref( "Vue 3 lesoon" );
    
  </script>
  
  <style lang="css" scoped>
  
  </style>