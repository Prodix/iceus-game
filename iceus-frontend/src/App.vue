<script setup>
  import Hero from './components/Hero.vue';
  import Quiz from './components/Quiz.vue';
  import Dialog from './components/Dialog.vue';
  import { ref, onMounted, watch, toRef } from 'vue';

  const hero = ref();
  const quiz = ref();
  const dialog = ref();
  const isQuizShowed = ref(false);
  const isSaying = ref(false);
  const isShowed = ref(false);

  let i = 0;

  const replies = [
    'Привет, дорогой абитуриент!',
    'Сегодня ты познакомишься с главными образовательными окрестностями Великого Новгорода!',
    'Пройдёшь увлекательный маршрут, получишь новые знания и интересные факты!',
    'Хорошего тебе пути, желаю успехов!',
  ];

  const questions = [
    'Этот монумент, воздвигнут в Великом Новгороде в 1862 году в честь тысячелетнего юбилея призвания варягов на Русь. Как он называется?',
    'Монумент, посвященный русскому поэту и первому министру юстиции, а также создателю первого неофициального гимна России. О каком монументе идет речь?',
    'Памятник, установленный у здания Новгородской технической школы, посвященный князю, который правил древним Новгородом - создателю первой школы повышенного типа, подразумевавшую «книжное учение». О каком памятнике идет речь?',
    'Упразднённый монастырь в Новгороде, не только один из центров духовной жизни Новгородской республики, но и начало формирования отечественной науки и образования. Основателем и первым настоятелем монастыря был преподобный Антоний Римлянин. Как он называется?',
    'Музей об истории развития письменности в Великом Новгороде. Его открытие было приурочено ко Дню славянской письменности и культуры. О каком музее идёт речь?',
    'Научное здание, основанное 13 декабря 1833 года, которое входит в архитектурный комплекс Новгородского Кремля. Является центром краеведческой библиографии. Как оно называется?'
  ];

  const currentQuestion = ref(questions[i]);

  const quizData = [
    new Map([
      ['Памятник Новгородскому ополчению', false],
      ['Памятник Тысячелетию России', true],
      ['Памятник Александру Невскому', false]
    ]),
    new Map([
      ['Бюст С.М. Кирову', false],
      ['Памятник В. И. Ленину', false],
      ['Монумент Г. Р. Державину', true]
    ]),
    new Map([
      ['Памятник Ярославу Мудрому', true],
      ['Памятник Владимиру Святославичу', false],
      ['Памятник Олегу Вещиму', false]
    ]),
    new Map([
      ['Хутынский монастырь', false],
      ['Николо-Вяжищский монастырь', false],
      ['Антониево монастырь', true]
    ]),
    new Map([
      ['Музей письменности', true],
      ['Музей изобразительных искусств', false],
      ['Музей культуры и письма', false]
    ]),
    new Map([
      ['Библиотека мировой художественной культуры и иностранных языков', false],
      ['Гимназия имени Героя Советского Союза, Почетного гражданина Новгорода И.А.Каберова', false],
      ['Новгородская областная универсальная научная библиотека', true]
    ])
  ];

  const currentQuiz = ref(quizData[i]);

  const positiveAnswers = ['Молодец! Продолжай в том же духе!', 'Хорошая работа!', 'Продолжай свой путь смело!'];
  const negativeAnswers = ['Подумай ещё раз!'];

  function onAnswerPicked(val) {
    const comment = document.getElementById('comment');
    const span = comment.querySelector('span');
    isShowed.value = true;

    if (val === true) {
      let index = Math.floor(Math.random() * positiveAnswers.length);
      isSaying.value = true;
      dialog.value.sayOne(positiveAnswers[index], 0).then(() => {
        setTimeout(() => {
          dialog.value.clearText();
          isSaying.value = false;
          if (i === quizData.length - 1) {
            isQuizShowed.value = false;
          } else {
            currentQuiz.value = quizData[++i];
            currentQuestion.value = questions[i];
          }
        }, 1000); 
      });
    }
    else {
      let index = Math.floor(Math.random() * negativeAnswers.length);
      isSaying.value = true;
      dialog.value.sayOne(negativeAnswers[index], 0).then(() => {
        setTimeout(() => {
          dialog.value.clearText();
          isSaying.value = false;
        }, 1000);
      });
    }
  }

  onMounted(() => {
    setTimeout(() => {
      isShowed.value = true;
    },2000);
  });

</script>

<template>
  <transition name="test">
    <Dialog v-show="isShowed" :replies="replies" ref="dialog" 
    v-model:isDialogShowed="isShowed" 
    v-model:isQuizShowed="isQuizShowed"/>
  </transition>  
  <Hero ref="hero"/>
  <transition name="transtwo">
    <Quiz v-show="isQuizShowed" @clicked="onAnswerPicked" ref="quiz" 
    :question="currentQuestion" 
    :answers="currentQuiz" 
    :isSaying="toRef(isSaying)"/>
  </transition>
</template>

<style scoped>
  .test-enter-active,
  .test-leave-active,
  .transtwo-enter-active,
  .transtwo-leave-active {
    transition: opacity 0.5s ease;
  }
  .test-enter-from,
  .test-leave-to,
  .transtwo-enter-from,
  .transtwo-leave-to  {
    opacity: 0;
  }
  
  
</style>
