<script setup>

import { ref } from 'vue'

const headerClassList = ref("header")

</script>

<template>
    <header :class="headerClassList">
        
    </header>
</template>

<style lang="scss">

.header {
}

</style>
