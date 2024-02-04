<script setup>

const props = defineProps ({
    value: String,
    isLong: {
        default: false,
        type: Boolean
    }
})

function onClick(e) {
    if(props.value === "Del") {
        document.dispatchEvent(new KeyboardEvent('keydown',{'key': 'Backspace'}))
    }
    else [    
        document.dispatchEvent(new KeyboardEvent('keydown',{'key':props.value}))
    ]
}
</script>

<template>
<div :class="['key', {'key--long' : props.isLong} ]" @click="onClick">
    <p class="key__p">
        {{ props.value }}
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
.key--long {
    width: 10rem;
}
.key:hover {
    background-color: #ffe7b3;
}
.key:active {
    background-color: #ffefcc;
}

</style>