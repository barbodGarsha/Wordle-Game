<script setup>
import TheHeader from './components/TheHeader.vue'
import Column from './components/Column.vue'
import Row from './components/Row.vue'
</script>

<template>
  <TheHeader></TheHeader>

  <Row>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
  </Row>
  
  <Row>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
  </Row>
  
  <Row>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
  </Row>
  
  <Row>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
  </Row>
  
  <Row>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
    <Column value="X"></Column>
  </Row>

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
