<script setup >

defineProps(['value'])

</script>

<template>
    <div class="column">
        <p class="column__input">{{ value }}</p>
    </div>
</template>

<style lang="scss">

    .column {
        background-color: gold;

        display: flex;
        justify-content: center;
        align-items: center;

        width: 5rem;
        height: 5rem;

        .column__input {
            font-size: 2rem;
        }   
    }

</style>