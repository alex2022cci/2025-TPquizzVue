<template>
    <div class="question">
        <h3>{{ question.question }}</h3>
        <ul>
            <li v-for="(choice, index) in randoChoices" :key="choice">
                <Answer 
                    :id="`answer${index}`"
                    :disabled="!hasAnswer"
                    :value="choice"
                    v-model="answer"
                    @change="onAnswer"
                    :correctAnswer="question.correct_answer"
                />
       
            </li>
        </ul>
        Votre réponse actuelle : {{ answer }}


    </div>
</template>

<script setup>
import { shuffleArray } from '@/functions/array'
import { computed, ref, onMounted, onUnmounted } from 'vue'
import Answer from './Answer.vue'

const props = defineProps({question: Object})
const emits = defineEmits(['answer'])
const answer = ref(null)
const hasAnswer = computed(() => answer.value === null)
const randoChoices = computed(() => shuffleArray(props.question.choices))
let timer 
const onAnswer = () => {
    clearTimeout(timer)
    timer = setTimeout(() => {
        emits('answer', answer.value)
    }, 1_500)
}

onMounted(() => {
    timer = setTimeout(() => {
        answer.value = ''
        onAnswer()
    }, 3_500)
})
onUnmounted(() => {
    clearTimeout(timer)
})

</script>

<style scoped>
    li{
        list-style: none;
    }
    .question{
        padding-top: 2rem;
        padding-bottom: 1rem;
        padding-left: 1rem;
        color-scheme: dark;
        background-color: #333;
        color: #fff;
        border-radius: 1rem;
    }
    .question button{
        margin-left: auto;
        display: block;
    }

</style>