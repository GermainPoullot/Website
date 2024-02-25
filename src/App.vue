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
    question: "En quelle année suis-je né ?",
    answers: ["1994", "1995", "1996", "1997", "1998"],
    answer: "1996",
    points: 0,
    id: 1,
},
{
    question: "Dans quelles villes ai-je principalement habité ?",
    answers: ["Levallois", "Meudon" ],
    questionType: "textList",
    points: 0,
    id: 2,
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
    question: "Plutôt thé ou café ?",
    answers: ["Thé", "Café", "Maté", "Chocolat chaud", "Infusion"],
    answer: "Thé",
    points: 0,
    id: 5,
},
{
    question: '"Le polytope des chemins monotones est une déformation du polytope de pivot"',
    answers: ["Cette phrase est vraie", "Cette phrase est écrite dans le mauvais sens"],
    answer: "Cette phrase est vraie",
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
},
{
    question: "La première phrase de ma thèse est (l'autre a été écrite par ChatGPT)",
    answers: ["Mathematicians study geometry for more than 2 500 years.",
              "In the realm of mathematics and its far-reaching applications, polytopes stand as fascinating geometric objects that have intrigued scholars for centuries."],
    answer: "Mathematicians study geometry for more than 2 500 years.",
    points: 0,
    id: 9,
},
{
    question: "L'un des paragraphes suivants a été écrit avec ChatGPT, l'autre provient de ma thèse. Lequel provient de ma thèse ?",
    answers: ["The aim is to optimize a linear functional subject to linear constraints. For example, imagine I want to breed goats and cows. My barn has 15 boxes, a goat takes 1 box and a cow 3 boxes. Milking a goat gives 4L, while a cow gives 3L, and my storage allows at most 24L. I want to maximize the number of animals I have.",
              "Imagine you have $100 to spend on teddy bears and toy cars. Teddy bears cost $10 each, and toy cars cost $5 each. You want to buy as many toys as possible without exceeding your budget of $100. However, you can only fit up to 10 teddy bears and 20 toy cars in your room. This is a linear programming problem, where you aim to maximize the total number of toys (teddy bears and toy cars) you can buy while staying within your budget and storage space constraints."],
    answer: "The aim is to optimize a linear functional subject to linear constraints. For example, imagine I want to breed goats and cows. My barn has 15 boxes, a goat takes 1 box and a cow 3 boxes. Milking a goat gives 4L, while a cow gives 3L, and my storage allows at most 24L. I want to maximize the number of animals I have.",
    points: 0,
    id: 10,
},
{
    question: "L'un des paragraphes suivants a été écrit avec ChatGPT, l'autre provient de ma thèse. Lequel provient de ma thèse ?",
    answers: ["Such a keen interest in polyhedra may be explained by their duality. Polytopes, particularly polyhedra, have captivated mathematicians, physicists, and researchers across various disciplines for centuries due to their unique property of being dual to one another. This duality not only reveals profound relationships between seemingly distinct geometric structures but also unveils essential insights into the underlying symmetries and combinatorial properties of polytopes. In this Ph.D. thesis, we delve deep into the rich world of polytopes, exploring their duality transformations, topological properties, and applications across diverse scientific domains.",
              "Such a keen interest for polyhedra may be explained by their duality. Indeed, polyhedra are both simple to define, and rich in the behaviors they can express; they are sitting on the fence between concrete numerical geometry and purely abstract topology; they are drawn and visualized by everyone but elementary properties can be hard to prove; they seem alighting from the realm of ideas, although they can be directly encountered in nature (from capsids of viruses to furnishing). If this has participated in draping polyhedra with a mystical allure, it has also exerted a prolific fascination on numerous mathematicians."],
    answer: "Such a keen interest for polyhedra may be explained by their duality. Indeed, polyhedra are both simple to define, and rich in the behaviors they can express; they are sitting on the fence between concrete numerical geometry and purely abstract topology; they are drawn and visualized by everyone but elementary properties can be hard to prove; they seem alighting from the realm of ideas, although they can be directly encountered in nature (from capsids of viruses to furnishing). If this has participated in draping polyhedra with a mystical allure, it has also exerted a prolific fascination on numerous mathematicians.",
    points: 0,
    id: 11,
},
{
    question: "Combien ai-je de T-shirts à motifs japonais?",
    answers: ["4", "8", "6", "2", "5", "42", "10", "12"],
    answer: "8",
    points: 0,
    id: 12,
},
{
    question: "Sur quel jeu ai-je passé le plus de temps ?",
    answers: ["Super Smash Bros. Ultimate", "Yoshi's Island", "Elden Ring", "Zelda: Tears of the Kindgdom", "The binding of Isaac: Repentance"],
    answer: "The binding of Isaac: Repentance",
    points: 0,
    id: 13,
},
{
    question: "Quel est le pseudonyme que j'utilise sur Internet quand ce n'est pas mon vrai nom ?",
    answers: ["Monsieur Poulpe", "Nathanaël Targaryen", "Germichou", "xXSniperDu92Xx", "Embrun_Forestier" ],
    answer: "Nathanaël Targaryen",
    points: 0,
    id: 14,
},
{
    question: "A quoi suis-je allergique ?",
    answers: ["Amande, Pistache, Noisette", "Cacahuète, Noix de cajou, Noix de pécan", "Noisette, Noix, Noix de pécan", "Noix du Brésil, Noix de Macadamia, Noix de muscade"],
    answer: "Noisette, Noix, Noix de pécan",
    points: 0,
    id: 15,
},
{
    question: "Dans quels pays suis-je allé faire des maths depuis mon M1 (inclus) ?",
    answers: ["Allemagne", "Russie", "Autriche", "France", "Italie", "Espagne"],
    questionType: "textList",
    id: 16,
    points: 0,
},
{
    question: "Combien de feuilles faut-il pour réaliser ce cube tronqué en origami ?",
    answers: ["3", "6", "7", "9", "12", "15", "30"],
    answer: "12",
    mainImagePath: "origami.jpeg",
    points: 0,
    id: 17,
},
{
    question: "Quel est le nom de ce polyèdre ?",
    answers: ["Prisme sur un segment", "Prisme sur un triangle",
              "Prisme sur un carré", "Prisme sur un tétraèdre",
              "Biprisme sur un segment", "Biprisme sur un triangle",
              "Biprisme sur un carré", "Biprisme sur un tétraèdre",
              "Pyramide sur un segment", "Pyramide sur un triangle",
              "Pyramide sur un carré", "Pyramide sur un tétraèdre",
              "Bipyramide sur un segment", "Bipyramide sur un triangle",
              "Bipyramide sur un carré", "Bipyramide sur un tétraèdre" ],
    answer: "Bipyramide sur un triangle",
    mainImagePath: "bipyramide.jpg",
    points: 0,
    id: 18,
},
{
    question: "Quel est le nom de ce polyèdre ?",
    answers: ["Octaèdre cubique", "Octaèdre rhombique",
              "Octaèdre tronqué", "Octaèdre régulier",
              "Décaèdre cubique", "Décaèdre rhombique",
              "Décaèdre tronqué", "Décaèdre régulier",
              "Dodécaèdre cubique", "Dodécaèdre rhombique",
              "Dodécaèdre tronqué", "Dodécaèdre régulier",
              "Icosaèdre cubique", "Icosaèdre rhombique",
              "Icosaèdre tronqué", "Icosaèdre régulier"],
    answer: "Dodécaèdre rhombique",
    mainImagePath: "dodecaedre_rhombique.jpg",
    points: 0,
    id: 19,
},
{
    question: "Je vais où après ma thèse (à partir du 25/10/2023) ?",
    answers: ["Chômage (France)", "Bologne (Italie)", "Barcelone (Espagne)",
              "Osnabrück (Allemagne)", "Londres (Angleterre)"],
    answer: "Osnabrück (Allemagne)",
    points: 0,
    id: 20,
},
{
    question: "Quel était le thème de littérature (philosophique) quand j'ai passé le concours des Grandes Écoles ?",
    answers: ["Le temps vécu", "La guerre", "Le monde des passions", "Servitude et soumission",
              "L'aventure", "L'amour"],
    answer: "Le monde des passions",
    points: 0,
    id: 21,
},
{
    question: "De quelle auteure ai-je lu plus de 14 000 pages ?",
    answers: ["Jane Austen", "Agatha Christie", "Marguerite Duras", "Robin Hobb",
              "Anne Robillard", "George Sand", "Virginia Woolf"],
    answer: "Robin Hobb",
    points: 0,
    id: 22,
},
{
    question: "Combien y-a-t'il d'arêtes dans un cube de dimension d ?",
    mainImagePath: "q1/cubes.png",
    answers: ["2^(d+1) - 2^d", "d*2^(d-1)", "2^d", "(2 parmi d) * 2^(d-1)"],
    imagesAnswers : [ "q1/2d2d.png", "q1/d2d.png", "q1/2d.png", "q1/binom.png"],
    answer: "d*2^(d-1)",
    id: 23,
    points: 0,
},
{
    question: "Quel est le nom du journal communal de Meudon ?",
    answers: ["Point d'Appui", "Le Meudonnais", "Chloroville", "Le Canard des Gardes",
             "Belle vue et Val-fleury"],
    answer: "Chloroville",
    points: 0,
    id: 24,
},
{
    question: "Parmi ces auteurs, lequel n'est pas cité dans ma thèse ?",
    answers: ["Virginia Woolf", "Oscar Wilde", "Walt Whitman", "Michel Berger", "Thomas Fuller",
              "Marie Curie", "Edgar Allan Poe"],
    answer: "Virginia Woolf",
    points: 0,
    id: 25,
},
{
    question: "Quelle est la dimension du cône sous-modulaire pour n = 4 (cône de déformation du permutaèdre de dimension 3), sans son linéal ?",
    answers: ["4", "9", "11", "14", "26"],
    answer: "11",
    points: 0,
    id: 26,
},
{
    question: "Combien ai-je suspendu de posters dans le couloir 15/16, 5ème étage ?",
    answers: ["0", "1", "2", "3", "4", "5"],
    answer: "3",
    points: 0,
    id: 27,
},
{
    question: "Combien y-a-t'il eu de séances du séminaire DGeCo (le résumé des séances est disponible sur mon site) ?",
    answers: ["10", "36", "42", "52", "59", "63", "74", "1216"],
    answer: "63",
    points: 0,
    id: 28,
},
{
    question: "Dans \"Le bateau ivre\" d'Arthur Rimbaud, de quoi est ivre la carcasse du bateau ?",
    answers: ["D'eau", "De vins bleus", "De sèves inouïes", "De noirs parfums", "D'amour", "De tristesse", "D'orgueil"],
    answer: "D'eau",
    points: 0,
    id: 29,
},
{
    question: "Avez-vous aimé ce petit jeu ?",
    answers: ["Ouiiiiiiiiiiiiiiiiiiiii", "Non"],
    answer: "Ouiiiiiiiiiiiiiiiiiiiii",
    points: 0,
    id: 30,
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
    console.log("Suivant");
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
                Liste des questions
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
