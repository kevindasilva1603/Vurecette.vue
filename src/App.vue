<template>

    <div id="app">

        <div v-if="index === questions.length">

            <p>Félicitations, vous avez terminé le quiz!</p>

        </div>

        <div v-else>

            <div class="question-container">

                <question-box
                    v-if="questions.length > 0"
                    :currentQuestion="questions[index]"
                    @nextQuestion="next"
                    @incrementScore="increment"
                />

            </div>

        </div>

    </div>

</template>

<script>
 import QuestionBox from "./components/Question.vue"; export default { name: "App",
components: { QuestionBox, }, data() { return { questions: [], index: 0, numCorrect:
0, total: 0, }; }, methods: { next() { if (this.index < this.questions.length) {
this.index++; } }, increment(isCorrect) { if (isCorrect) { this.numCorrect++; } this.total++;
}, }, mounted() { fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple")
.then(response => response.json()) .then(data => { this.questions = data.results;
this.total = data.results.length; }); }, };
</script>

<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.question-container {
    max-width: 600px;
    margin: auto;
}
</style>

