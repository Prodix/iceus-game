<script setup>
    import { ref, watch, onMounted } from 'vue';

    const text = ref('');
    const speed = ref(30);
    const isSaying = ref(false);
    const isNextTextShown = ref(false);
    let i = 0;

    const sayReply = (replies) => {
        setTimeout(() => {
            const nextReply = () => {
                if (i < replies.length && !isSaying.value) {
                    isSaying.value = true;
                    isNextTextShown.value = false;
                    clearText();
                    say(replies[i], 0).then(() => {
                        i++;
                        isSaying.value = false;
                        isNextTextShown.value = true;
                    });
                } else if (!isSaying.value) {
                    closeDialog();
                    document.removeEventListener('click', nextReply);
                }
            };

            nextReply();
            document.addEventListener('click', nextReply);
        }, 1000);
    };

    const sayEndReply = (replies, param) => {
        let h = 0;
        document.getElementById('comment').style.width = '32.3%';
        setTimeout(() => {
            return new Promise(resolve => {
                const nextReply = () => {
                    if (h < replies.length && !isSaying.value) {
                        isSaying.value = true;
                        isNextTextShown.value = false;
                        clearText();
                        say(replies[h], 0).then(() => {
                            h++;
                            isSaying.value = false;
                            param.value = true;
                        });
                    } else if (!isSaying.value) {
                        props.isDialogShowed = false;
                        emit('update:isDialogShowed', false);
    
                        isNextTextShown.value = false;
                        document.removeEventListener('click', nextReply);
                    }
                };
    
                nextReply();
            })
        }, 500);
    };

    const sayOne = (txt, i) => {
        return new Promise(function (resolve, reject) {
            if (i < txt.length) {
                text.value += txt[i];
                i++;
                setTimeout(() => sayOne(txt, i).then(resolve, reject), speed.value);
            } else {
                setTimeout(() => {
                    resolve();
                }, 500);
            }
        });
    }

    const say = (txt, i) => {
        return new Promise(function (resolve, reject) {
            if (i < txt.length) {
                text.value += txt[i];
                i++;
                setTimeout(() => say(txt, i).then(resolve, reject), speed.value);
            } else {
                resolve();
            }
        });
    }

    const clearText = () => {
        text.value = '';
    }

    const closeDialog = () => {
        props.isDialogShowed = false;
        emit('update:isDialogShowed', false);

        isNextTextShown.value = false;
        
        setTimeout(() => {
            clearText();
            props.hero.changeHeroState('default');
            props.isQuizShowed = true;
            emit('update:isQuizShowed', true);
            return true;
        }, 1000);
    }

    defineExpose({
        sayOne,
        closeDialog,
        clearText,
        sayReply,
        sayEndReply
    });

    const props = defineProps([
        'replies',
        'isDialogShowed',
        'isQuizShowed',
        'hero'
    ]);

    const emit = defineEmits([
        'update:isDialogShowed',
        'update:isQuizShowed'
    ]);

    onMounted(() => {
        window.onresize = (event) => {
            const comment = document.getElementById("comment");
            const hero = document.getElementById("hero");
            comment.style.left = hero.width - (hero.width / 3) + 'px';
            comment.style.bottom = hero.height - (hero.height / 5) + 20 + 'px';
        };

        const comment = document.getElementById("comment");
        const hero = document.getElementById("hero");
        comment.style.left = hero.width - (hero.width / 3) + 'px';
        comment.style.bottom = hero.height - (hero.height / 5) + 40 + 'px';
        
        setTimeout(() => {
            sayReply(props.replies);
        }, 700);
    });
    

</script>


<template>
    <div id="comment">
        <p>{{ text }}</p>
        <transition name="next">
            <span v-show="isNextTextShown">Далее</span>
        </transition>
    </div>
</template>


<style scoped>
    @font-face {
        font-family: 'MP Manga';
        src: url('../assets/MP Manga.ttf');
        font-weight: normal;
        font-style: normal;
        font-size: 20px;
    }

    @media screen and (max-width: 1200px) {
        p, span {
            font-size: 17px;
        }
    }

    * {
        user-select: none;
    }
    #comment{
        width: 19.3%;
        background-color: white;
        position: relative;
        border-radius: 10px;
        position: absolute;
        left: 10%;

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
    #comment > span {
        color: black;
        font-family: 'MP Manga';
        position: absolute;
        right: 10px;
        bottom: 0;
    }
    p {
        color: black;
        padding: 15px;
        word-break: break-word;
        text-align: start;
        font-family: 'MP Manga';
        transform-origin: top;
    }
    .next-enter-active,
    .next-leave-active {
        transition: opacity 0.5s ease;
    }
    .next-enter-from,
    .next-leave-to {
        opacity: 0;
    }
</style>