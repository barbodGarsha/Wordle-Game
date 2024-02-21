<script setup>

const emit = defineEmits(['clicked'])

const props = defineProps ({
    isIconOnly: {
        default: false,
        type: Boolean
    },
    icon: {
        default: null,
        type: Image
    },
    hasRotaionAnimation: {
        default: false,
        type: Boolean
    },
    hasMirrorAnimation: {
        default: false,
        type: Boolean
    }
})


function clicked() {
    emit('clicked')
}

</script>

<template>
    <div @click="clicked" :class="['btn', {'btn--has-rotation-animation' : props.hasRotaionAnimation}, {'btn--has-mirror-animation' : props.hasMirrorAnimation}]">
        <img v-if="props.isIconOnly" :src="props.icon" alt="icon" class="btn__icon">
    </div>
</template>

<style lang="scss">
    .btn {
        cursor: pointer;

        display: flex;
        justify-content: center;
        align-self: center
        ;
        background-color: #FFF9EC;
        
        border-radius: 15px;
        width: 4rem;
        .btn__icon {
            pointer-events: none;
            width: 100%;

            padding: .75rem;

            transition-duration: 500ms;
        }
    }
    .btn--has-mirror-animation {
        .btn__icon {
            transition-duration: 1s;
        }
    }
    .btn--has-rotation-animation:hover {
        .btn__icon {
            transform: rotate(-180deg);
        }
    }
    .btn--has-mirror-animation:hover {
        .btn__icon {
            transform: rotateY(360deg);
        }
    }
    
</style>