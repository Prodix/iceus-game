<script setup>
    import { ref } from 'vue';

    const props = defineProps({
        question: String,
        answers: Map,
        isSaying: Object
    });

    const emit = defineEmits([
        'clicked'
    ]);

    function checkAnswer(event) {
        if (props.isSaying.value === true) return;

        if (props.answers.get(event.target.textContent) === true) {
            emit('clicked', true);
            event.target.classList.add("correct");
            setTimeout(() => {
                event.target.classList.remove("correct");
            }, 1000);
        } else {
            emit('clicked', false);
            event.target.classList.add("incorrect");
            setTimeout(() => {
                event.target.classList.remove("incorrect");
            }, 1000);
        }
    }

    defineExpose({
        props
    });
</script>

<template>
    <div id="quiz">
        <p>{{ props.question }}</p>
        <ul>
            <li v-for="answer in props.answers.keys()">
                <button @click="checkAnswer">{{ answer }}</button>
            </li>
        </ul>
    </div>
</template>

<style scoped>
    * {
        user-select: none;
    }
    #quiz {
        position: absolute;
        width: 60%;
        left: 63%;
        transform: translateX(-50%);
        bottom: 0;
    }
    #quiz > p {
        background-color: white;
        color: black;
        padding: 10px;
        border-radius: 10px;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        margin: 10px 0 10px 0;
    }
    button {
        background-color: white;
        color: black;
        border: none;
        padding: 10px;
        width: 100%;
        border-radius: 10px;
        font-family: MP Manga;
        transition: background-color 1s;
    }
    .correct {
        transition: 1s;
        color: white;
        background-color: green;
    }
    .incorrect {
        transition: 1s;
        color: white;
        background-color: red;
    }
    @media screen and (max-width: 1200px) {
        p, span, button {
            font-size: 17px;
        }
    }
</style>

