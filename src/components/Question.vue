<template>

    <div class="question-box-container">

        <div class="jumbotron">

            <h1 class="display-4">{{ currentQuestion.question }}</h1>

            <hr class="my-4" />

            <div class="list-group">

                <a
                    v-for="(answer, index) in shuffledAnswers"
                    :key="index"
                    class="list-group-item list-group-item-action"
                    @click="selectAnswer(index)"
                    :class="{ 'bg-info': selectedIndex === index, 'bg-success': answered && index === correctIndex, 'bg-danger': answered && index !== correctIndex && selectedIndex === index }"
                >
                    {{ answer }}
                </a>

            </div>

            <button
                class="btn btn-primary mt-3"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                Soumettre
            </button>

            <button class="btn btn-success mt-3" @click="next">Suivant</button>

        </div>

    </div>

</template>

<script>
 export default { props: { currentQuestion: Object, next: Function, increment: Function,
}, data() { return { selectedIndex: null, correctIndex: null, answered: false, shuffledAnswers:
[], }; }, watch: { currentQuestion: { immediate: true, handler() { this.answered
= false; this.selectedIndex = null; this.correctIndex = null; this.shuffleAnswers();
}, }, }, methods: { shuffleAnswers() { const answers = [...this.currentQuestion.incorrect_answers,
this.currentQuestion.correct_answer]; for (let i = answers.length - 1; i > 0; i--)
{ const j = Math.floor(Math.random() * (i + 1)); [answers[i], answers[j]] = [answers[j],
answers[i]]; } this.shuffledAnswers = answers; }, selectAnswer(index) { if (!this.answered)
{ this.selectedIndex = index; } }, submitAnswer() { this.answered = true; this.correctIndex
= this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer); this.increment(this.selectedIndex
=== this.correctIndex); }, }, };
</script>

<style scoped>
.selected {
    background-color: lightblue !important;
}
.correct {
    background-color: lightgreen !important;
}
.incorrect {
    background-color: red !important;
}
</style>

