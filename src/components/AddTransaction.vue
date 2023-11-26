<template>
     <h3>Add new product</h3>
      <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text">Product name</label>
          <input type="text" id="text" v-model="productName" placeholder="Enter text..." />
        </div>
        <div class="product">
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (in gramm)</label>
          <input type="text" id="amount" v-model="amount" placeholder="Enter wight..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Cost<br />
            (in Euro)</label>
          <input type="text" id="amount" v-model="cost" placeholder="Enter cost..." />
        </div>
      </div>
        <button class="btn">Add transaction</button>
      </form>
</template>

<script setup>
import {ref} from 'vue';
import {useToast} from 'vue-toastification';

const productName = ref('');
const amount = ref('');
const cost = ref('');

const toast = useToast()
// custom event we can emit are defined like this
//this can passed to this component to another one
//A custom event pretty much just like 'submit' just custom
const emit = defineEmits(['transactionSubmitted'])

const onSubmit = () =>{
  if(!productName.value || !amount.value || !cost.value){
      toast.error('All fields must be filled');
      return;
  }
  const transactionData = {
      productName:  productName.value,
      amount: parseInt(amount.value) ,
      cost: parseFloat(cost.value)
  }

// this is what actualy emits the info
emit('transactionSubmitted', transactionData);
  productName.value = '';
  amount.value = '';
  cost.value = '';
}

</script>