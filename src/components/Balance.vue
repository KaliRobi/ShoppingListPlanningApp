<template>
<div class="budget-setting">

<div class="budget-setting-element">
<label><h4>Your budget</h4></label>
<h2 id="balance"> {{ budget }}€</h2>
</div >

<div class="budget-setting-element">
    <form>
    <label for="text"><h4>Enter Your budget</h4></label>
    
    <input type="text" id="text" 
    v-model="budget"
    @input="handleChange(budget)"
    placeholder="Today spendings..." />
    </form>
</div>
</div>
</template>
<script setup>
import { ref } from 'vue'

const budget  = ref('')
const emit = defineEmits(['currentBudget'])

const handleChange = (budget) => {
    emit('currentBudget', parseFloat(budget))
    }


</script>