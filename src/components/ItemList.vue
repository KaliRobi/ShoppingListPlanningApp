<!-- composition api -->
<template>
 <h3>Shopping list draft</h3>
    <ul id="list" class="list">
        <!-- directive -->
        <li 
        v-for="shoppingItem in shoppingItems" 
        :key="shoppingItem.id" 
        :class="shoppingItem.amount < 0? 'minus' : 'plus'  " >
            {{ shoppingItem.productName }} <span>{{ shoppingItem.amount }} g</span> <span>{{ shoppingItem.cost }}€</span>
            <button @click="deleteshoppingItem(shoppingItem.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>

const emit = defineEmits(['shoppingItemDeleted'])

const props = defineProps({
    shoppingItems: {
        type: Array,
        required: true
    }
});
// again, emiting the custom event and its props
const deleteshoppingItem = (id) => {
        emit('shoppingItemDeleted', id);
}

</script>
