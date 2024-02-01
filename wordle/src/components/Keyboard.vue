
<script setup>

import Key from './Key.vue'

</script>

<template>
    <div class="keyboard">
        <div class="keyboard__row">
            <Key>Q</Key>
            <Key>W</Key>
            <Key>E</Key>
            <Key>R</Key>
            <Key>T</Key>
            <Key>Y</Key>
            <Key>U</Key>
            <Key>I</Key>
            <Key>O</Key>
            <Key>P</Key>
        </div>
        <div class="keyboard__row keyboard__row--second">
            <Key>A</Key>
            <Key>S</Key>
            <Key>D</Key>
            <Key>F</Key>
            <Key>G</Key>
            <Key>H</Key>
            <Key>J</Key>
            <Key>K</Key>
            <Key>L</Key>
        </div>
        <div class="keyboard__row keyboard__row--third">
            <Key>Z</Key>
            <Key>X</Key>
            <Key>C</Key>
            <Key>V</Key>
            <Key>B</Key>
            <Key>N</Key>
            <Key>M</Key>
        </div>
    </div>
</template>

<style lang="scss">

    .keyboard {
        .keyboard__row {
            display: flex;
        }   
        .keyboard__row--second {
            margin-left: 2.5rem;
        }
        .keyboard__row--third {
            margin-left: 5rem;
        }
    }

</style>