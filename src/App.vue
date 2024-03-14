<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="+income"  :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted ='handleTransactionsSubmitted' />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import {useToast} from "vue-toastification"; 
import { ref, computed,onMounted } from 'vue';

const toast = useToast();   

const transactions = ref([ ]);

onMounted(()=>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if(savedTransactions){
    transactions.value = savedTransactions;
  }
});
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

// Add transaction
const handleTransactionsSubmitted =(transactionData)=>{
// console.log(transactionData);
 transactions.value.push({
  id:generateuniqueId(),
  text:transactionData.text,
  amount:transactionData.amount
 });

saveTransactionToLocalStoreage();
 toast.success("transaction added sucessfully")
};

// generate unique id
const generateuniqueId=()=>{
  return Math.floor(Math.random()*10000)
};

// delete transaction

const handleTransactionDeleted =(id)=>{
  console.log(id);
  transactions.value= transactions.value.filter((transaction)=>
  transaction.id !== id
  );

  saveTransactionToLocalStoreage();
  toast.success('transaction deleted');
}

// saved to localstorage

const saveTransactionToLocalStoreage =()=>{
  localStorage.setItem('transactions',JSON.stringify(transactions.value));
}

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
