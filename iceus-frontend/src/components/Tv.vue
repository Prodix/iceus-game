<script setup>
    import tv from '../assets/tv.gif';
    import ym from '../assets/ym.jpg';
    import books from '../assets/books.png';
    import writing from '../assets/writing.png';
    import thousand from '../assets/thousand.png';
    import monastir from '../assets/monastir.jpg';
    import derzhavin from '../assets/derzhavin.jpg';
    import { ref } from 'vue';

    const currentImage = ref(tv);
    let counter = 0;
    const imageList = [ thousand, derzhavin, ym, monastir, writing, books ];

    const maximize = () => {
        currentImage.value = imageList[counter++];
        const schema = document.getElementById('tv');
        schema.style.width = '100vw';
        schema.style.left = '0';
        schema.style.top = '0';
        schema.style.height = '100vh';
    }

    const minimize = () => {
        const schema = document.getElementById('tv');
        schema.style.width = '';
        schema.style.left = '';
        schema.style.top = '';
        schema.style.height = '';
        setTimeout(() => {
            currentImage.value = tv;
        }, 2000);
    }

    defineExpose({
        maximize,
        minimize
    });
</script>

<template>
    <div id="tv">
        <img :src="currentImage">
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
        transition: width 2s, height 2s, left 2s, top 2s;
    }
    #tv > img{
        width: 100%;
        height: 100%;
    }
</style>