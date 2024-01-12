<script setup >
import { watch, ref } from 'vue'

const props = defineProps({
    value: String,
    isCorrect: Boolean,
    isNotRightPos: Boolean,
    isWrong: Boolean
})

const classModifier = ref('')

watch(() => [props.isCorrect, props.isNotRightPos, props.isWrong], ([isCorrect, isNotRightPos, isWrong]) => {
    if(isCorrect) {
        classModifier.value = 'column--correct'
    }
    else if(isNotRightPos) {
        classModifier.value = 'column--not-right-position'
    }
    else if(isWrong) {
        classModifier.value = 'column--wrong'
    }
})

</script>

<template>
    <div :class="['column', classModifier]">
        <p class="column__input">{{ props.value }}</p>
    </div>
</template>

<style lang="scss">

    .column {
        background-color: beige;

        display: flex;
        justify-content: center;
        align-items: center;

        width: 5rem;
        height: 5rem;

        .column__input {
            font-size: 2rem;
        }   
    }
    .column--correct {
        background-color: green;
    }
    .column--not-right-position {
        background-color: yellow;
    }
    .column--wrong {
        background-color: gray;
    }

</style>