<script setup>
import TheHeader from './components/TheHeader.vue'
</script>

<template>
  <TheHeader></TheHeader>
  
  <main>
  </main>
</template>

<style lang="scss">
  
  //RESETS
  html {
      box-sizing: border-box;
  }
    
  *, *:before, *:after {
      box-sizing: inherit;
  }

  body {
      background-color: white;
      margin: 0;
      padding: 0;
      font-family: sans-serif;
  }
  a, li, h1, h2, p {
      text-decoration: none;
      margin: 0;
      padding: 0;
      z-index: 9999;
  }

  ul {
      display: inline;  
      margin: 0;
      padding: 0;
  }
  li {
      display: inline;
      list-style-type: none;
      margin: 0;
      padding: 0; 
  }
</style>
