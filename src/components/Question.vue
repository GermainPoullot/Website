/* Script setup that takes a question (string) and a list of answers (list) */
<script setup>
import { reactive, ref } from 'vue'
const props = defineProps({
        question: Object,
})
const selectedAnswer = ref(null)
const answerWasSubmitted = ref(false)
const localQuestion = ref(props.question.question)
const localAnswers = ref(props.question.answers)
const localProposals = ref(null)
const validAnswer = ref(props.question.answer)
const id = ref(props.question.id)
const ans1 = ref("")
const textAnswered = ref("Bravo !!")
const emit = defineEmits(['points', 'next'])

function selectAnswer(answer) {
    console.log(answer);
    this.selectedAnswer = answer;
}

function triggerNext() {
    emit('next');
}

function reset(newQuestion) {
    console.log("Reset");
    console.log(newQuestion.question)
    localQuestion.value = newQuestion.question;
    if (newQuestion.questionType === "textList") {
        localProposals.value = newQuestion.answers.map((answer) => {
            return {answer, inputA:""};
        })
        localAnswers.value = null;
    } else {
        localAnswers.value = newQuestion.answers;
    }
    validAnswer.value = newQuestion.answer;
    answerWasSubmitted.value = false;
    id.value = newQuestion.id;

}

defineExpose({
    reset
});

function submitAnswer(selectedAnswer) {
    console.log(selectedAnswer);
    var points = 0;
    textAnswered.value = "Dommage :( Essaie encore !";
    if (selectedAnswer == validAnswer.value) {
        points = 100;
        textAnswered.value = "Bravo !!";
    }
    emit('points', this.id, points);
    answerWasSubmitted.value = true;
}
</script>

<template>
    <div class=qt v-if="!answerWasSubmitted">
        <h1>{{ localQuestion }}</h1>
            <div v-for="answer in localAnswers" :key="answer" :class="{ 'answer-box': true, 'selected': selectedAnswer === answer }" @click=selectAnswer(answer)>
                {{ answer }}
            </div>
        <div class="qtManualText">
            <input v-for="answer in localProposals" type="text" :id=answer v-model=answer.inputA name="name" required minlength="4" maxlength="16" size="10" />
            {{ ans1 }}
        </div>
        <button v-if="selectedAnswer" @click=submitAnswer(selectedAnswer)>Submit</button>
    </div>
    <div class=answered v-else>
        <h1> {{ textAnswered }}</h1>
        <button @click=triggerNext>Bring me to next!</button>
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
  width: 200px;
  height: 100px;
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

</style>