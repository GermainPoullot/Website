<script setup>
import Question from './components/Question.vue'
import { reactive, ref } from 'vue'

var currentQuestion = 0;
const finished = ref(false);
const questions = [
{
    question: "Combien ai-je de doigts ?",
    answers: ["5", "10", "20", "30"],
    answer: "20",
    points: 0,
    id: 0,
},
{
    question: "Combien y-a-t'il d'arêtes dans un cube de dimension d ?",
    answers: ["2^(d+1) - 2^d", "d*2^(d-1)", "2^d", "(2 parmi d) * 2^(d-1)"],
    answer: "d*2^(d-1)",
    id: 1,
    points: 0,
},
{
    question: "Dans quels pays suis-je allé faire des maths depuis mon M1 (inclus) ?",
    answers: ["Allemagne", "Russie", "Autriche", "France"],
    questionType: "textList",
    id: 2,
    points: 0,
},
{
    question: "De quel pays n'ai-je pas lu de roman ?",
    answers: ["Nigeria", "Sénégal", "Congo", "Colombie"],
    answer: "Congo",
    points: 0,
    id: 3,
},
{
    question: "Quel est le premier jeu vidéo auquel j'ai joué tout seul ?",
    answers: ["Super Mario 64", "Zelda: Ocarina of Time", "Yoshi's Island", "Pokémon Bleu", "Sonic the Hedgehog"],
    answer: "Yoshi's Island",
    points: 0,
    id: 4,
},
{
    question: "En quelle année suis-je né ?",
    answers: ["1995", "1996", "1997", "1998"],
    answer: "1996",
    points: 0,
    id: 5,
},
{
    question: '"Le polytope des chemins monotones est une déformation du polytope de pivot"',
    answers: ["Cette phrase est vraie", "Cette phrase est écrite dans le mauvais sens"],
    answer: ["Cette phrase est vraie"],
    points: 0,
    id: 6,
},
{
    question: "Parmi les 5 solides de Platon, combien possèdent une projection vers un hexagone ?",
    answers: ["0", "1", "2", "3", "4", "5"],
    answer: "4",
    points: 0,
    id: 7,
},
{
    question: "Parmi les 5 solides de Platon, combien possèdent une projection vers un quadrilatère ?",
    answers: ["0", "1", "2", "3", "4", "5"],
    answer: "3",
    points: 0,
    id: 8,
}
]

const counter = ref(0)

const activeQuestion = ref(null);

const pointResult = (id, points) => {
    console.log(points, id, questions);
    if (questions[id] && questions[id].points === 0) {
        counter.value += points;
        questions[id].points = points;
    } else if (questions[id].points != 100) {
        const diff = points - questions[id].points
        if (diff > 0) {
            counter.value += diff;
            questions[id].points = points;
        }
    } else if (questions[id] == undefined) {
        console.log("Question with id not found ", id);
    }
}

function gotoNext() {
    console.log("Next");
    if (currentQuestion >= questions.length - 1) {
        currentQuestion = 0;
        finished.value = true;
        // TODO: Stop, print confettis and final score
    }
    activeQuestion.value.reset(questions[++currentQuestion]);
}

function goToQuestion(id) {
    finished.value = false;
    activeQuestion.value.reset(questions[id]);
    currentQuestion = id;
}

</script>

<template>
    <main>
        <div class="container">
            <div class="sidebar">
                <!-- Content for the left column goes here -->
                Question List
                    <div v-for="question in questions" :key="question.id" @click=goToQuestion(question.id) :class="{ 'full-points': question.points == 100, 'somePoints': question.points < 100 && question.points > 0,'zeroPoints': question.points === 0}">
                    Question {{ question.id }}
                </div>
            </div>
            <!-- Create a question saying "What is your name?" and proposing three names -->
            <div class="main-content">
                <div class="globalCounter">
                    <h2>Counter : {{ counter }}</h2>
                </div>

                <Question v-if="!finished" ref="activeQuestion" :question=questions[0] @points=pointResult @next=gotoNext />
                <div v-else>
                    Merci d'avoir participé ! Votre score final est de {{ counter }} points.
                </div>
            </div>
        </div>
    </main>
</template>

<style scoped>

.globalCounter {
    text-align: center;
}
.container {
    display: flex;
}

.sidebar {
    width: 100px; /* Adjust the width as needed */
    min-width: 100px;
    height: 100vh; /* Adjust the height as needed */
    overflow-y: scroll; /* Enable vertical scrolling */
    top: 0;
    bottom: 0;
}

.main-content {
    flex-grow: 1; /* Allow the main content to grow to fill remaining space */
}

.zeroPoints {
    background-color: hsl(0, 100%, 50%);
}

.somePoints {
    background-color: hsl(64, 100%, 70%);
}

.full-points {
    background-color: hsl(120, 100%, 50%);
}

@media (min-width: 1024px) {
    .logo {
        margin: 0 2rem 0 0;
    }

}
</style>
