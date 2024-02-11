<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import {useToast} from 'vue-toastification'
const toast = useToast()

import {computed, ref, onMounted} from 'vue';
/* const transactions = ref ([
    {id: 1, text: 'Flower', amount: -19.99},
    {id: 2, text: 'Salary', amount: 219.99},
    {id: 3, text: 'Book', amount: -109.99},
    {id: 4, text: 'Camera', amount: 419.99},
]) */

const transactions = ref([])

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransaction) {
    transactions.value = savedTransaction
  }
})

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0).toFixed(2)
})

const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount >0 )
  .reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0).toFixed(2)
})

const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0 )
  .reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0).toFixed(2)
})

const handleTransactionSubmitted = (transactionData) => {
  console.log(transactionData);
  transactions.value.push({
    id: generatedUniqueID(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  saveTransactionToLocalStorage()
  toast.success('Transaction added')
}

const generatedUniqueID = () => {
  return Math.floor((Math.random() * 1000000))
}

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
  saveTransactionToLocalStorage()
  toast.success('Transaction deleted')
}

const saveTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
  <Header></Header>

  <main>
    <div class="container">
      <Balance :total="+total"></Balance>
      <IncomeExpenses :income="+income" :expenses="+expenses"></IncomeExpenses>
      <TransactionList :transactions="transactions" @deleteTransaction="handleTransactionDeleted"></TransactionList>
      <AddTransaction @transactionSubmitted="handleTransactionSubmitted"></AddTransaction>
    </div>
  </main>
</template>

<style scoped>

</style>
