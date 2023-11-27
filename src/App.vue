<template>
<Header />
    <div class="container">
            <Balance 
            @currentBudget="updateBudget"/>
            <BalanceTracker :spentBudget="+spentBudget" :availableBudget="+availableBudget"/>
            <ItemList
            @shoppingItemDeleted="handleDeleted"
            :shoppingItems="shoppingItems"/>
            <AddItem
             @shoppingItemSubmitted="handleSubmitted" />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import BalanceTracker from './components/BalanceTracker.vue';
import ItemList from './components/ItemList.vue';
import AddItem from './components/AddItem.vue';
import {ref, computed, onMounted} from 'vue';
import {useToast} from 'vue-toastification';

const toast = useToast();
const  shoppingItems= ref([]);
const availableBudget = ref('')


onMounted(() => {
    const savedTransaction = JSON.parse(localStorage.getItem('shoppingItems'))
    if(savedTransaction){
        shoppingItems.value = savedTransaction;
    }
})

const  assignAvailableBudget = (res) => {
     availableBudget.value = res
}

const followBudgetChange = (change) => {
    availableBudget.value += change
}

const updateBudget = (currentBudget) => {
    const currentSpendings = shoppingItems.value.reduce((acc, trans) => {
    return acc + trans.cost }, 0 );
    return assignAvailableBudget(parseFloat( currentBudget - currentSpendings )) 
}


//should be the budget - the money still left
const spentBudget = computed(() => {
    return  shoppingItems.value
    .reduce((acc, trans) => {
        return acc + trans.cost;
    }, 0 )
})


// the custom event caught here
const handleSubmitted = (transactionData) =>{
        shoppingItems.value.push({
            id: generateUniqueId(),
            productName: transactionData.productName,
            amount: transactionData.amount,
            cost : transactionData.cost
        });
        updateBudget(availableBudget.value)
        saveShoppingItemsToStorage();
        toast.success('Transaction added')
}

const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000000)
}


const handleDeleted = (id) => {
    var deletedCost;
        shoppingItems.value.forEach( item => {
            if(item.id == id){
                deletedCost = item.cost
            }
        })
        shoppingItems.value = shoppingItems.value.filter(transaction => 
        transaction.id !== id
    )    
        
    followBudgetChange(deletedCost)
    saveShoppingItemsToStorage();
    toast.success('transaction deleted')
}



const saveShoppingItemsToStorage = () => {
    localStorage.setItem('shoppingItems', JSON.stringify(shoppingItems.value))
}

</script>