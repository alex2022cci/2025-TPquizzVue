<template>

 <div class="container">
     <div v-if="state === 'error'">
        <p>Erreur lors du chargement du quizz</p>
     </div>
     
     <div :aria-busy="state === 'loading'" >
        <Quizz :quiz="quiz" v-if="quiz" />
     </div>
 </div>


</template>

<script setup>
import { ref, onMounted } from 'vue' ;
import Quizz from './Components/Quizz.vue'

const quiz = ref(null)
const state = ref('loading')

onMounted(() => {
    fetch('./quiz.json')
    .then(r => {
        if (r.ok) {
            return r.json()
        }
        throw new Error('Impossible de trouver le .json')
    })
    .then(data => {
        quiz.value = data
        state.value = 'idle'
    })
    .catch(err => {
    console.error(err)
    state.value = 'error'
    })
})
</script>

<style>
.container {
    margin-top: 2rem;
}
</style>
