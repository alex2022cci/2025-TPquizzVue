<template>
                   <label :for="id"
                            :class="classes"
                   >
                    <input :id="id" 
                            type="radio" 
                            name="answer"
                            :value="value"
                            :disabled="disabled"
                            @change="onChange"
                            v-model="model"
                            >
                    {{ value }}
                </label>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    id: String,
    disabled: Boolean,
    value: String,
    correctAnswer: String
})
const emits = defineEmits(['change'])
const onChange = (e) => {
    emits('change', e)
}
const model = defineModel()
const classes = computed(() => ({
    disabled: props.disabled,
    right:  props.disabled && props.value === props.correctAnswer,
    wrong:  props.disabled && props.value !== props.correctAnswer
            && model.value === props.value
}))
</script>

<style scoped>
.right{
    color: green  ;
    opacity: 1;
}
.wrong{
    color: red  ;
    opacity: 1;
}
.disabled{
    opacity: 0.5;
    pointer-events: none;
}
</style>