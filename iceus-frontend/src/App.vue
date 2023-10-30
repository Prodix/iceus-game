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
    hero.value.say(replies[0], 0).then(result => {
      isSaying = false;
    })
    document.addEventListener('click', () => {
      if (i < replies.length && !isSaying) {
        isSaying = true;
        hero.value.text = '';
        hero.value.say(replies[i], 0).then(result => {
          isSaying = false;
          i++;
        });
      }
    });
  });
</script>

<template>
  <Hero ref="hero"/>
</template>

<style scoped>

</style>
