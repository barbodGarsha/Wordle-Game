<script setup>

</script>

<template>
    <div class="overlay">
        <div class="overlay__container">
            <slot>

            </slot>
        </div>
    </div>
</template>

<style lang="scss">

    .overlay {
        background-color: rgba($color: #000000, $alpha: .5);

        display: flex;
        justify-content: center;
        align-items: center;

        position: absolute;
        width: 100vw;
        height: 100vh;

        top: 0;
        left: 0;
        z-index: 999999;

        .overlay__container{
            background-color: #FFF9EC;
            
            display: flex;
            justify-content: center;
            align-items: center;
            
            border-radius: 15px;

            padding: 1rem;
            width: 40rem;
            height: 45rem;

        }
    }

</style>