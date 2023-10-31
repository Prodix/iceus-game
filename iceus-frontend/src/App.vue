<script setup>
  import Hero from './components/Hero.vue'
  import { ref, onMounted } from 'vue';

  const hero = ref();

  let replies = ['Привет, дорогой абитуриент!', 
'Сегодня ты познакомишься с главными образовательными окрестностями Великого Новгорода!', 'Пройдёшь увлекательный маршрут, получишь новые знания и интересные факты!', 
'Хорошего тебе пути, желаю успехов!'];

  let i = 1;
  let isSaying = true;

  onMounted(() => {
    const comment = document.getElementById("comment");
    const span = document.querySelector("#comment > span");
    const heroImage = document.getElementById("hero");
    hero.value.isHidden = false;
    setTimeout(() => {
      comment.classList.add('showed');
      heroImage.classList.add('showed');
      hero.value.say(replies[0], 0).then(result => {
        span.classList.add('showed');
        setTimeout(() => {
            isSaying = false;
          }, 500);
      });
    }, 2000); 
    document.addEventListener('click', () => {
      if (i < replies.length && !isSaying) {
        span.classList.remove('showed');
        isSaying = true;
        hero.value.text = '';
        hero.value.say(replies[i], 0).then(result => {
          span.classList.add('showed');
          setTimeout(() => {
            isSaying = false;
            i++;
          }, 500);
        });
      }
      else if (i >= replies.length) {
        comment.classList.remove('showed');
        setTimeout(() => {
          hero.value.isHidden = true;
          hero.value.text = '';
        }, 2000);
      }
    });
  });
</script>

<template>
  <Hero ref="hero"/>
</template>

<style scoped>

</style>
