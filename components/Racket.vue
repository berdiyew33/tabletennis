<template>
        <svg width="100" height="200" viewBox="-100 -100 200 200" :style="`transform: translate(${props.positionX}, ${props.positionY})`" ref="racket">
            <g :transform="'rotate(' + racketRotation + ')'" stroke-width="5" stroke="black">
                <path d="M-12 100 L -12 50 A 75,75 0,1,1 12 50 L 12 100 Z" :fill="props.color"/>
                <path d="M-59 20 L 59 20"/>
            </g>
        </svg>
</template>
<script setup>
const props = defineProps({
    positionX: {
        type: String,
        default: 'calc(50vw - 50px)'
    },
    positionY: {
        type: String,
        default: 'calc(50vh - 420px)'
    },
    color: {
        type: String,
        default: '#ff0000'
    }
})
const racket = ref(null);
defineExpose({
    racket
});
const racketRotation = ref(0);
if (props.color === '#ff0000') {
    racketRotation.value = 0;
} else {
    racketRotation.value = 180;
}   
</script>
<style scoped>
    svg{
        touch-action: none;
        position: absolute;
        z-index: 2;
    }
</style>