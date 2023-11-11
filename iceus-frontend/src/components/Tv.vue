<script setup>
    import tv from '../assets/tv.gif';
</script>

<template>
    <div id="tv">
        <img :src="tv">
    </div>
</template>


<style scoped>
    #tv {
        position: absolute;
        z-index: -2;
        left: 29.55%;
        top: 37.15%;
        height: 14.5%;
        width: 14.7%;
    }
    #tv > img{
        width: 100%;
        height: 100%;
    }
</style>