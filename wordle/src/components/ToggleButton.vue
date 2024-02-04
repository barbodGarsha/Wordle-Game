<script setup>
import { ref } from 'vue'

const emit = defineEmits(['toggled'])

const props = defineProps ({
    enabled: {
        default: false,
        type: Boolean
    }
})

const isEnabled = ref(props.enabled)

function clicked(e) {
    isEnabled.value = !isEnabled.value
    emit('toggled', isEnabled.value)
}
</script>

<template>
    <div @click="clicked" :class="['toggle-btn', { 'toggle-btn--enabled' : isEnabled }]">
        <div class="toggle-btn__circle">
        </div>
    </div>
</template>

<style lang="scss">
    .toggle-btn {
        cursor: pointer;
        background: #1E4652;
        display: flex;
        justify-content: flex-start;
        align-items: center;

        border-radius: 1.5rem;
        width: 5rem;
        height: 3rem;
        .toggle-btn__circle {
            background-color: #FFF9EC;

            margin-left: .1rem;
            border-radius: 50%;
            width: 2.6rem;
            height: 2.6rem;
            transition-duration: 500ms;
        }
    }
    .toggle-btn--enabled {
        .toggle-btn__circle {
            margin-left: 2.3rem;
        }
    }
</style>