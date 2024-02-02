
<script setup>

import Key from './Key.vue'

</script>

<template>
    <div class="keyboard">
        <div class="keyboard__row">
            <Key value="Q"></Key>
            <Key value="W"></Key>
            <Key value="E"></Key>
            <Key value="R"></Key>
            <Key value="T"></Key>
            <Key value="Y"></Key>
            <Key value="U"></Key>
            <Key value="I"></Key>
            <Key value="O"></Key>
            <Key value="P"></Key>
            <Key value="Del" is-long="true"></Key>
        </div>
        <div class="keyboard__row keyboard__row--second">
            <Key value="A"></Key>
            <Key value="S"></Key>
            <Key value="D"></Key>
            <Key value="F"></Key>
            <Key value="G"></Key>
            <Key value="H"></Key>
            <Key value="J"></Key>
            <Key value="K"></Key>
            <Key value="L"></Key>
            <Key value="Enter" is-long="true"></Key>
        </div>
        <div class="keyboard__row keyboard__row--third">
            <Key value="Z"></Key>
            <Key value="X"></Key>
            <Key value="C"></Key>
            <Key value="V"></Key>
            <Key value="B"></Key>
            <Key value="N"></Key>
            <Key value="M"></Key>
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
            margin-left: 10rem;
        }
    }

</style>