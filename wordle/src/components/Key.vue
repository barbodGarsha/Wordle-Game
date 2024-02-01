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
    background-color: #FFF9EC;

    display: flex;
    justify-content: center;
    align-items: center;

    border: solid 2px white;

    width: 5rem;
    height: 5rem;

    .key__p {
        font-size: 2rem;
    }   
}
.key:hover {
    background-color: #ffe7b3;
}
.key:active {
    background-color: #ffefcc;
}

</style>