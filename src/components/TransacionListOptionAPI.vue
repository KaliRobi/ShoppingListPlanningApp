<template>
    <h3>History</h3>
       <ul id="list" class="list">
           <!-- directive -->
           <li 
           v-for="transaction in transactions" 
           :key="transaction.id" 
           :class="transaction.amount < 0? 'minus' : 'plus'  " >
               {{ transaction.text }} <span>{{ transaction.amount }}€</span><button class="delete-btn">x</button>
           </li>
       </ul>
   </template>
   
   <script>
   export default{
       //from here we can access it in the html
       data(){
           return{
               transactions: [
                   {id: 1, text: 'Flower', amount: -19.99},
                   {id: 2, text: 'Salary', amount: 299.97},
                   {id: 3, text: 'book', amount: -19.99},
                   {id: 4, text: 'Camera', amount: 200},
               ]
           }
       }
   }
   
   </script>