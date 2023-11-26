<!-- composition api -->
<template>
 <h3>Shopping list draft</h3>
    <ul id="list" class="list">
        <!-- directive -->
        <li 
        v-for="transaction in transactions" 
        :key="transaction.id" 
        :class="transaction.amount < 0? 'minus' : 'plus'  " >
            {{ transaction.productName }} <span>{{ transaction.amount }} g</span> <span>{{ transaction.cost }}€</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>

const emit = defineEmits(['transactionDeleted'])

const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
});
// again, emiting the custom event and its props
const deleteTransaction = (id) => {
        emit('transactionDeleted', id);
}

</script>
