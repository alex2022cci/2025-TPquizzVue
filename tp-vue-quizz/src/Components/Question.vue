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
                    :correctAnswer="question.correct_answer"
                />
       
            </li>
        </ul>
        Votre réponse actuelle : {{ answer }}
        <button :disabled="hasAnswer" @click="emit('answer', answer)">Question suivante</button>
    </div>
</template>

<script setup>
import { shuffleArray } from '@/functions/array'
import { computed, ref } from 'vue'
import Answer from './Answer.vue'

const props = defineProps({question: Object})
const emit = defineEmits(['answer'])
const answer = ref(null)
const hasAnswer = computed(() => answer.value === null)
const randoChoices = computed(() => shuffleArray(props.question.choices))
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