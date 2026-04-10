<template>
    <svg width="50" height="50"  :style="`transform: translate(${props.positionX}, ${props.positionY})`">
        <circle cx="25" cy="25" :r="props.radius" fill="white" stroke="black" stroke-width="5"/>
    </svg>
</template>

<script setup>
    const props = defineProps({
        radius:{
            type: Number,
            default: 15
        },
        positionX:{
            type:String,
            default: 'calc(50vw - 25px)'
        },
        positionY:{
            type:String,
            default: 'calc(50vh + 150px)'
        }
    })
</script>
<style>
    #ball{
        transform: translate(calc(50vw - 25px), calc(50vh + 150px));
    }
</style>