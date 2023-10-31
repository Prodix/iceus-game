<script setup>
    import hero from "../assets/hero.png";
    import { ref, onMounted, watch } from 'vue';

    const text = ref('');
    const speed = 50;
    const isHidden = ref(true);

    const say = (txt, i) => {
        return new Promise(function (resolve, reject) {
            if (i < txt.length) {
                text.value += txt[i];
                i++
                setTimeout(() => say(txt, i).then(resolve, reject), 50);
            }
            else {
                resolve();
            }
        });
    }

    onMounted(() => {
        const comment = document.getElementById("comment");
        const hero = document.getElementById("hero");
        comment.style.left = hero.width + 'px';
        comment.style.top = -comment.clientHeight + 'px';
    });

    watch(text, newText => {
        const comment = document.getElementById("comment");
        comment.style.top = -comment.clientHeight + 'px';
    });

    defineExpose({
        say,
        text,
        isHidden
    });
    
</script>

<template>
    <div id="hero-container">
        <div>
            <img id="hero" :src="hero" alt="hero"/>
            <div v-show="!isHidden" id="comment">
                <p>{{ text }}</p>
                <span>Далее</span>
            </div>
        </div>
    </div>
</template>

<style scoped>
    @font-face {
        font-family: 'MP Manga';
        src: url('../assets/MP Manga.ttf');
        font-weight: normal;
        font-style: normal;
    }
    img {
        width: 300px;
        user-select: none;
        opacity: 0;
        transition: opacity .5s;
    }
    img.showed {
        opacity: 1;
    }
    #comment {
        width: 300px;
        max-height: 400px;
        background-color: white;
        position: relative;
        border-radius: 10px;
        position: absolute;
        transition: opacity 1s;
        opacity: 0;

        &::before {
            content: '';
            position: absolute;
            bottom: -5px;
            left: -15px;
            transform: rotate(245deg);
            border-width: 20px 0 0 20px; 
            border-style: solid;
            border-color: transparent transparent transparent white; 
        }
    }
    #comment.showed {
        opacity: 1;
    }
    #comment > span {
        color: black;
        font-family: 'MP Manga';
        position: absolute;
        right: 10px;
        bottom: 0;
        opacity: 0;
        transition: opacity 0s;
    }
    #comment > span.showed {
        opacity: 1;
        transition: opacity .2s;
    }
    p {
        color: black;
        padding: 15px;
        word-break: break-word;
        text-align: start;
        font-family: 'MP Manga';
        transform-origin: top;
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
</style>