<script setup>

const ICONS_FOLDER_PATH = '../assets/icons/'

const props = defineProps ({
    isIconOnly: {
        default: false,
        type: Boolean
    },
    iconName: {
        default: '',
        type: String
    },
    hasRotaionAnimation: {
        default: false,
        type: Boolean
    }
})

function getImageUrl() {
  return new URL(ICONS_FOLDER_PATH + props.iconName, import.meta.url)
}

</script>

<template>
    <div :class="['btn', {'btn--has-rotation-animation' : props.hasRotaionAnimation}]">
        <img v-if="props.isIconOnly" :src="getImageUrl()" alt="icon" class="btn__icon">
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
        width: 5rem;
        .btn__icon {
            width: 100%;

            padding: 1rem;

            transition-duration: 500ms;
        }
    }
    .btn--has-rotation-animation:hover {
        .btn__icon {
            transform: rotate(180deg);
        }
    }
</style>