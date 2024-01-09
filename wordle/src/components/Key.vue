<script setup>

</script>

<template>
<div class="key">
    <p class="key__p">
        <slot></slot>
    </p>
</div>
</template>

<style lang="scss">

.key {
    cursor: pointer;
    background-color: chartreuse;

    display: flex;
    justify-content: center;
    align-items: center;

    width: 5rem;
    height: 5rem;

    .key__p {
        font-size: 2rem;
    }   
}

</style>