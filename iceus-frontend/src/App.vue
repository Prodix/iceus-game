<script setup>
  import Hero from './components/Hero.vue';
  import Quiz from './components/Quiz.vue';
  import Dialog from './components/Dialog.vue';
  import Schema from './components/Schema.vue';
  import Tv from './components/Tv.vue';
  import Info from './components/Info.vue';
  import qr from './assets/qr.png';
  import background from './assets/background.png';
  import { ref, onMounted, watch, toRef } from 'vue';

  const hero = ref();
  const aud = ref();
  const quiz = ref();
  const tv = ref();
  const dialog = ref();
  const info = ref();
  const isQuizShowed = ref(false);
  const isSaying = ref(false);
  const isShowed = ref(false);
  const schema = ref();
  const isStarted = ref(false);
  const isInfoShowed = ref(false);
  const isQrShown = ref(false);
  let isNextButtonClicked = false;
  let counter = 0;
  let counter2 = 0;
  let counter3 = 0;

  let i = 0;

  const replies = [
    'Привет, дорогой абитуриент!',
    'Сегодня ты познакомишься с главными образовательными окрестностями Великого Новгорода!',
    'Пройдёшь увлекательный маршрут, получишь новые знания и интересные факты!',
    'Хорошего тебе пути, желаю успехов!',
  ];

  const end = [
    'Я директор, Трифонов Владимир Александрович, приглашаю на обучение в наше учебное заведение!  Институт ведет подготовку по образовательным программам среднего профессионального образования, бакалавриата, магистратуры и аспирантуры на очной и очно-заочной формах обучения. Для более подробной информации по специальностям и направлениям подготовки необходимо зайти на наш официальный сайт по QR-коду.',
  ];

  const questions = [
    'Монумент, посвященный русскому поэту и первому министру юстиции, а также создателю первого неофициального гимна России',
    'Упразднённый монастырь в Новгороде, не только один из центров духовной жизни Новгородской республики, но и начало формирования отечественной науки и образования. Основателем и первым настоятелем монастыря был преподобный Антоний Римлянин. Как называется этот монастырь?',
    'Памятник, установленный у здания Новгородской технической школы, посвященный князю, который правил древним Новгородом - создателю первой школы повышенного типа, подразумевавшую «книжное учение»',
    'Научное здание, основанное 13 декабря 1833 года, которое входит в архитектурный комплекс Новгородского Кремля. Является центром краеведческой библиографии. Как оно называется?',
    'Монумент, который воздвигнут в Великом Новгороде в 1862 году в честь тысячелетнего юбилея призвания варягов на Русь.',
    'Музей истории развития письменности в Великом Новгороде. Его открытие было приурочено ко Дню славянской письменности и культуры. О каком музее идёт речь?',
    'Институт, деятельность которого направлена на развитие экономического образования, науки, развитие экономики региона путем проведения фундаментальных и прикладных исследований и обучения на всех уровнях среднего, высшего и дополнительного образования по широкому спектру цифровых и экономических направлений?',
  ];

  const currentQuestion = ref(questions[i]);

  const quizData = [
    new Map([
      ['Бюст С.М. Кирову', false],
      ['Памятник В. И. Ленину', false],
      ['Монумент Г. Р. Державину', true]
    ]),
    new Map([
      ['Хутынский монастырь', false],
      ['Николо-Вяжищский монастырь', false],
      ['Антониев монастырь', true]
    ]),
    new Map([
      ['Памятник Ярославу Мудрому', true],
      ['Памятник Владимиру Святославичу', false],
      ['Памятник Олегу Вещиму', false]
    ]),
    new Map([
      ['Библиотека мировой художественной культуры и иностранных языков', false],
      ['Гимназия имени Героя Советского Союза, Почетного гражданина Новгорода И.А.Каберова', false],
      ['Новгородская областная универсальная научная библиотека', true]
    ]),
    new Map([
      ['Памятник Новгородскому ополчению', false],
      ['Памятник Тысячелетию России', true],
      ['Памятник Александру Невскому', false]
    ]),
    new Map([
      ['Музей письменности', true],
      ['Музей изобразительных искусств', false],
      ['Музей культуры и письма', false]
    ]),
    new Map([
      ['Институт Медицинского образования', false],
      ['Санкт-Петербургский государственный экономический университет - Новгородский филиал', false],
      ['Институт цифровой экономики, управления и сервиса', true]
    ]),
  ];

  const currentQuiz = ref(quizData[i]);

  const positiveAnswers = ['Молодец! Продолжай в том же духе!', 'Хорошая работа!', 'Продолжай свой путь смело!'];
  const negativeAnswers = ['Подумай ещё раз!', 'Это неправильный ответ, попробуй снова!'];

  const waitPromise = (time) => new Promise(resolve => setTimeout(resolve, time));
  const waitForClick = (event) => new Promise(async resolve => {
    isNextButtonClicked = false;
    document.addEventListener('click', (event) => {
      isNextButtonClicked = true;
    });
    while (!isNextButtonClicked) {
      await waitPromise(500);
    }
    isNextButtonClicked = false;
    document.removeEventListener('click',(event) => {
      isNextButtonClicked = true;
    });
    info.value.isNextTextShown = false;
    resolve();
  });

  async function onAnswerPicked(val) {
    isShowed.value = true;

    if (val === true) {
      counter2 = 0;
      let index = Math.floor(Math.random() * positiveAnswers.length);
      isSaying.value = true;
      hero.value.changeHeroState('accept');
      if (counter3 === 6) {
        await dialog.value.sayOne('Уважаемый абитуриент, да, действительно это Институт цифровой экономики, управления и сервиса!', 0);
      } else {
        await dialog.value.sayOne(positiveAnswers[index], 0);
      }
      counter3++;
      await waitPromise(1000);
      dialog.value.closeDialog();
      await waitPromise(1000);
      hero.value.changeHeroVisibility(false);
      dialog.value.clearText();
      isQuizShowed.value = false;
      let tvd = document.getElementById('tv');
      tvd.style.zIndex = '4';
      isInfoShowed.value = true;
      tv.value.maximize();
      await info.value.showNextInfo();
      await waitForClick();
      isInfoShowed.value = false;
      tv.value.minimize();
      await waitPromise(2500);
      info.value.text = '';
      tvd.style.zIndex = '-2';
      if (counter3 !== 7) {
        schema.value.maximize();
        schema.value.showNextPath();
        await waitPromise(3000);
        schema.value.minimize();
        await waitPromise(2000);
      }
      if (i !== quizData.length - 1) {
        isShowed.value = true;
        await dialog.value.sayOne('Переходим к следующему вопросу', 0);
        await waitPromise(500);
        isShowed.value = false;
        dialog.value.clearText();
      }
      if (i !== quizData.length - 1) {
        currentQuiz.value = quizData[++i];
        currentQuestion.value = questions[i];
        isQuizShowed.value = true;
        isSaying.value = false;
      } else {
        isShowed.value = true;
        await dialog.value.sayEndReply(end, isQrShown);
      }
    }
    else {
      hero.value.changeHeroState('wrong');
      let index = Math.floor(Math.random() * negativeAnswers.length);
      if (counter2 !== 0) {
        isSaying.value = true;
        dialog.value.sayOne(negativeAnswers[index], 0).then(() => {
          setTimeout(() => {
            dialog.value.closeDialog();
            setTimeout(() => {
              dialog.value.clearText();
              isSaying.value = false;
            }, 1000);
          }, 1000);
        });
        counter2++;
      }
      else {
        isSaying.value = true;
        dialog.value.sayOne('У тебя есть ещё две попытки, смелее!', 0).then(() => {
          setTimeout(() => {
            dialog.value.closeDialog();
            setTimeout(() => {
              dialog.value.clearText();
              isSaying.value = false;
            }, 1000);
          }, 1000);
        });
        counter2++;
      }
    }
  }

  onMounted(() => {
    aud.value.volume = 0.025;
  });

</script>

<template>
  <img :src="background" id="back">
  <div id="startscreen" v-if="!isStarted">
    <button id="startbutton" @click="isStarted = true">Начать</button>
  </div>
  <div v-if="isStarted">
    <Schema ref="schema"/>
    <Tv ref="tv"/>
    <Hero ref="hero" v-model:isDialogShowed="isShowed"/>
    <transition name="test">
      <Dialog v-show="isShowed" :replies="replies" ref="dialog"
      v-model:isDialogShowed="isShowed" 
      :hero="hero"
      v-model:isQuizShowed="isQuizShowed"/>
    </transition>  
    <transition name="transtwo">
      <Quiz v-show="isQuizShowed" @clicked="onAnswerPicked" ref="quiz" 
      :question="currentQuestion" 
      :answers="currentQuiz" 
      :isSaying="toRef(isSaying)"/>
    </transition>
    <transition name="test">
      <Info v-show="isInfoShowed" ref="info"/>
    </transition>
    <transition name="test">
      <div id="qr" v-show="isQrShown">
        <img width="250" :src="qr">
      </div>
    </transition>
  </div>
  <audio autoplay loop ref="aud">
    <source src="./assets/audio.mp3" type="audio/mpeg" />
  </audio>
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
  
  #back {
    position: absolute;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;
    z-index: -10;
  }

  * {
    user-select: none;
    overflow: hidden;
  }

  #startscreen {
    background-color: rgba(0, 0, 0, 0.6);
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #startbutton {
    width: 150px;
    height: 50px;
    font-family: 'MP Manga';
    border: 3px solid green;
    border-radius: 10px;
    background-color: white;
    color: black;
    font-size: 20px;
  }

  #qr {
    background-color: white;
    border-radius: 10px;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    z-index: 20;
    bottom: 2%;
    display: flex;
    justify-content: center;
    align-items: center;
    box-shadow: 0 0 8px #000000d1;
  }

</style>
