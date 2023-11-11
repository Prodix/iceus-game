<script setup>
    import hero_welcome from "../assets/hero_welcome.png";
    import hero_default from "../assets/hero_default.png";
    import hero_wrong from "../assets/hero_wrong.png";
    import hero_accept from "../assets/hero_accept.png";
    import { ref, onMounted, watch } from 'vue';
    
    const showHero = () => {
        let image = document.getElementById('hero');
    }

    const isHeroShown = ref(false);
    const heroType = ref('welcome');

    const changeHeroState = (state) => {
        setTimeout(() => {
            isHeroShown.value = false;
            setTimeout(() => {
                heroType.value = state;
                isHeroShown.value = true;
            }, 150);
        }, 150);
    };

    const changeHeroVisibility = (visibility) => {
        const heroElement = document.getElementById('hero');
        heroElement.style.zIndex = visibility === false ? '-4' : '0';
    }

    onMounted(() => {
        setTimeout(() => {
            isHeroShown.value = true;
        }, 500);
    });

    defineExpose({
        changeHeroState,
        changeHeroVisibility
    });
</script>

<template>
    <transition name="hero">
        <div id="hero-container" v-show="isHeroShown">
            <div>
                <img id="hero" :src="heroType === 'default' ? hero_default : heroType === 'wrong' ? hero_wrong : heroType === 'accept' ? hero_accept : hero_welcome" alt="hero"/>
            </div>
        </div>
    </transition>
</template>

<style scoped>
    img {
        width: 550px;
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