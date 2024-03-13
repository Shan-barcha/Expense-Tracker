<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="income"  :expenses="expenses"/>
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';

const transactions = ref([
  { id: 1, text: 'flower', amount: -19.9 },
  { id: 1, text: 'salary', amount: 4009.9 },
  { id: 1, text: 'Camera', amount: -200.9 },
  { id: 1, text: 'Books', amount: 299.9 },
]);
// get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
  
});
// get income 
const income = computed(() => {
  return transactions.value
  .filter((transaction)=> transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
  
});
// get expense 
const expenses = computed(() => {
  return transactions.value
  .filter((transaction)=> transaction.amount<  0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
  
});
// export default {
//   components: {
//     Header,
//     Balance,
//     IncomeExpense,
//     TransactionList,
//     AddTransaction,
//   },
// };
</script>
