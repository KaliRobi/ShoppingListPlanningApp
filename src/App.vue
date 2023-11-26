<template>
<Header />
    <div class="container">
            <Balance 
            @currentBudget="updateBudget"/>
            <BalanceTracker :spentBudget="+spentBudget" :availableBudget="availableBudget"/>
            <TransactionList
            @transactionDeleted="handleDeleted"
            :transactions="transactions"/>
            <AddTransaction
             @transactionSubmitted="handleSubmitted" />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import BalanceTracker from './components/BalanceTracker.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import {ref, computed, onMounted} from 'vue';
import {useToast} from 'vue-toastification';

const toast = useToast();
const  transactions= ref([]);


onMounted(() => {
    const savedTransaction = JSON.parse(localStorage.getItem('transactions'))
    if(savedTransaction){
        transactions.value = savedTransaction;
    }
})


const updateBudget = (currentBudget) => {
    if(transactions.value.length > 0 ){
   const currentSpendings = transactions.value.reduce((acc, trans) => {
    return acc + trans.cost }, 0 ).toFixed(2);
    console.log(parseFloat( currentBudget - currentSpendings ))
return parseFloat( currentBudget - currentSpendings ) }
return currentBudget;
}


//should be the budget - the money still left
const spentBudget = computed(() => {
    return  transactions.value
    .reduce((acc, trans) => {
        return acc + trans.cost;
    }, 0 )
})

//budget - sum of the transactions
const availableBudget = updateBudget();


// the custom event catched here
const handleSubmitted = (transactionData) =>{
        transactions.value.push({
            id: generateUniqueId(),
            productName: transactionData.productName,
            amount: transactionData.amount,
            cost : transactionData.cost
        });
        saveTransactionsToStorage();
        toast.success('Transaction added')
}

const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000000)
}

const handleDeleted = (id) => {
        transactions.value = transactions.value.filter(transaction => 
        transaction.id !== id
    )
    saveTransactionsToStorage();
    toast.success('transaction deleted')
}



const saveTransactionsToStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}



// to register the component 
// export default{
//     components:{
//         Header,
//         Balance,
//         IncomeExpenses,
//         TransactionList,
//         AddTransaction
//     },
// }
</script>