<template>
<div class="inc-exp-container">
        <div>
          <h4>Spent Euros</h4>
          <p id="money-minus" class="money minus">{{ spentBudget }}€</p>
        </div>
        <div>
          <h4>Left in budget</h4>
          <p id="money-plus" class="money plus">{{ availableBudget }}€</p>
        </div>
      </div>
</template>

<script setup>

const affairs =  defineProps({
  availableBudget : {
      type: Number,
      requred: true
    },
    spentBudget: {
      type: Number,
      requred:true
    }
})



</script>