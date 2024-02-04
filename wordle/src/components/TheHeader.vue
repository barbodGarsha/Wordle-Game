<script setup>

import { ref } from 'vue'

//TODO: the const classes like the ones below should be hard coded into the template but the modifiers better be using refs instead

const props = defineProps ({
    darkMode: {
        default: false,
        type: Boolean
    }
})
</script>

<template>
    <header :class="['header', {'header--darkmode' : props.darkMode}]">
        <p class="header__title">Wordle</p>
    </header>
</template>

<style lang="scss">

.header {
    background-color: #FFF9EC;
    
    display: flex;
    justify-content: center;
    align-items: center;

    width: 100vw;
    
    padding: 2rem;

    .header__title {
        color: #1E4652;
 
        text-transform: uppercase;
 
        font-size: 2rem;
        font-weight: 700;
        letter-spacing: 1rem;
    }
}
.header--darkmode {
    background-color: #1E4652;
    .header__title {
        color: #FFF9EC;
    }
}

</style>
