<template>
    <h1>Recap</h1>
    <p>
        {{ hasFinish ? quizz.success_message : quizz.failure_message }}
    </p>
    <p>Vous avez eu {{ score }} bonnes réponses sur {{ quizz.questions.length }}</p>
</template>

<script setup>
import { computed, defineProps } from 'vue'

const props = defineProps({
    answers: Array,
    quizz: Object
})

const score = computed(() => {
    return props.quizz.questions.reduce((acc, question , k) => {
        if(question.correct_answer === props.answers[k]){
            return acc + 1
        }
        return acc
    }, 0)
})
const hasFinish = computed(() => score.value >= props.quizz.minimum_score)
</script>