/* Script setup that takes a question (string) and a list of answers (list) */
<script setup>
import { reactive, ref } from 'vue'
const props = defineProps({
        question: Object,
})
const selectedAnswer = ref(null)
const answerWasSubmitted = ref(false)

const localQuestion = ref(props.question.question)
const mainImagePath = ref(null)

const localAnswers = ref(props.question.answers)
const localImagesAnswers = ref(null)
const textAnswers = ref(null)
const localProposals = ref(null)
const QuestionType = {
 	MultiChoice: 0,
 	TextInput: 1,
}
const questionType = ref(QuestionType.MultiChoice)
const validAnswer = ref(props.question.answer)
const id = ref(props.question.id)
const ans1 = ref("")
const textAnswered = ref("Bravo !!")
const emit = defineEmits(['points', 'next'])

function selectAnswer(answer) {
    console.log(answer);
    selectedAnswer.value = answer;
}

function triggerNext() {
    emit('next');
}

function reset(newQuestion) {
    console.log("Reset");
    console.log(newQuestion.question)
    selectedAnswer.value = null;
    mainImagePath.value = newQuestion.mainImagePath;
    localQuestion.value = newQuestion.question;
    localImagesAnswers.value = newQuestion.imagesAnswers;
    if (newQuestion.questionType === "textList") {
        questionType.value = QuestionType.TextInput;
        localProposals.value = newQuestion.answers.map((_) => {
            return {inputField:""};
        });
        textAnswers.value = newQuestion.answers.map((answer) => {
            return answer.toUpperCase();
        })
        localAnswers.value = null;
    } else {
        questionType.value = QuestionType.MultiChoice;
        localAnswers.value = newQuestion.answers;
        localProposals.value = null;
    }
    validAnswer.value = newQuestion.answer;
    answerWasSubmitted.value = false;
    id.value = newQuestion.id;

}

defineExpose({
    reset
});

function submitAnswer(selectedAnswer) {
    console.log("Submitting answer ", selectedAnswer, questionType.value);
    var points = 0;
    textAnswered.value = "Dommage :( Essaie encore !";
    if (questionType.value === QuestionType.MultiChoice) {
        console.log("MultiChoice answer", selectedAnswer, validAnswer.value)
        if (selectedAnswer === validAnswer.value) {
            points = 100;
        }
    } else {
        console.log("Text answer")
        const pointsPerAnswer = 100 / textAnswers.value.length;
        var correctAnswers = []
        localProposals.value = localProposals.value.map((element) => {
            return element.inputField.replace(/\s+/g, '').toUpperCase();
        });
        // Delete duplicates in localProposals
        localProposals.value = [...new Set(localProposals.value)]
        localProposals.value.forEach(element => {
            console.log(element);
            if (textAnswers.value.includes(element)) {
                points += pointsPerAnswer;
                correctAnswers.push(element.charAt(0).toUpperCase() + element.slice(1).toLowerCase());
            }
        });
        textAnswered.value = "Dommage :( Essaie encore ! Tu as trouvé : " + correctAnswers.join(", ");
    }
    if (points == 100) {
        textAnswered.value = "Bravo !!";
    }
    emit('points', id.value, points);
    answerWasSubmitted.value = true;
}
</script>

<template>
    <div class=qt v-if="!answerWasSubmitted">
        <h1>{{ localQuestion }}</h1>
        <img class="mainImage" v-if="mainImagePath" :src="`./${mainImagePath}`" >
            <div v-for="(answer, index) in localAnswers" :key="answer" :class="{ 'answer-box': true, 'selected': selectedAnswer === answer }" @click=selectAnswer(answer)>
                <div v-if="!localImagesAnswers">{{ answer }}</div> <img v-else class=answerImage :src="`./${localImagesAnswers[index]}`">
            </div>
        <div class="qtManualText">
            <input v-for="answer in localProposals" type="text" :id=answer v-model=answer.inputField name="name" class="textBox" required minlength="4" maxlength="16" size="10" />
            {{ ans1 }}
        </div>
        <button v-if="selectedAnswer || questionType == QuestionType.TextInput" @click=submitAnswer(selectedAnswer) class="boutonValider">Valider</button>
    </div>
    <div class=answered v-else>
        <h1> {{ textAnswered }}</h1>
        <button @click=triggerNext>Question suivante !</button>
    </div>
</template>

<style scoped>

.qt {
    padding: 1rem;
    margin: auto;
    text-align: center;
}
.selected {
  background-color: hsl(214, 83%, 70%);
}
.answer-box {
/*  width: 200px;
  height: 100px; */
  width: 75%;
  border: 1px solid black;
  margin: auto;
  margin-bottom: 10px;
  padding: 10px;
  cursor: pointer;
}
.answered {
    padding: 1rem;
    margin: auto;
  text-align: center;
  }

.boutonValider {
    width: 200px;
    height: 50px;
}

.textBox {
    margin: 10px;
}

.mainImage {
    display: block;
    max-height: 500pt;
    max-width: 75%;
    text-align: center;
    margin: auto;
    margin-bottom: 20pt;
}

.answerImage {
    height: 30px;
}

</style>