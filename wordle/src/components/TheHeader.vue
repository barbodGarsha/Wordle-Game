<script setup>

import { ref } from 'vue'

//TODO: the const classes like the ones below should be hard coded into the template but the modifiers better be using refs instead
const headerClassList = ref("header")
const header__titleClassList = ref("header__title")

</script>

<template>
    <header :class="headerClassList">
        <p :class="header__titleClassList">Wordle</p>
    </header>
</template>

<style lang="scss">

.header {
    background-color: beige;
    
    display: flex;
    justify-content: center;
    align-items: center;

    width: 100vw;
    
    padding: 2rem;

    .header__title {
        font-size: 2rem;
    }
}

</style>
