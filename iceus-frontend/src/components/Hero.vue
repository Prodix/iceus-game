<script setup>
    import hero from "../assets/hero.png";
    import { ref, onMounted, watch } from 'vue';
    
    const showHero = () => {
        let image = document.getElementById('hero');
    }

    const isHeroShown = ref(false);

    onMounted(() => {
        setTimeout(() => {
            isHeroShown.value = true;
        }, 500);
    });
</script>

<template>
    <transition name="hero">
        <div id="hero-container" v-show="isHeroShown">
            <div>
                <img id="hero" :src="hero" alt="hero"/>
            </div>
        </div>
    </transition>
</template>

<style scoped>
    img {
        width: 300px;
        user-select: none;
    }
    #hero-container {
        position: absolute;
        bottom: 0;
        left: 0;
    }
    #hero-container > div {
        position: relative;
        display: flex;
    }
    .hero-enter-active,
    .hero-leave-active {
        transition: opacity 0.5s ease;
    }
    .hero-enter-from,
    .hero-leave-to {
        opacity: 0;
    }
</style>