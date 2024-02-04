<script setup>

const emit = defineEmits(['backgroundClicked'])

const props = defineProps({
    hidden: {
        default: false,
        type: Boolean
    }
})

function backgroundClicked(e) {
    if(e.target.classList.contains('overlay')) { emit('backgroundClicked') }
}
</script>

<template>
    <div @click="backgroundClicked" :class="['overlay', {'overlay--hidden' : props.hidden}]">
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

        transition-duration: 200ms;
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
    .overlay--hidden {
        pointer-events: none;
        opacity: 0;
    }

</style>