<template>
  <Header />
  <CatFetch />
  <Balance :total="+total" />
  <IncomeExpenses :expenses="+expenses" :income="+income" />
  <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
  <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
</template>

<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import CatFetch from './components/CatFetch.vue'

import { ref, computed } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([
  { id: 1, text: 'flower', amount: -1233 },
  { id: 2, text: 'salary', amount: 299 },
  { id: 3, text: 'books', amount: -10 },
  { id: 4, text: 'camera', amount: 150 },
])

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

// Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  })
  toast.success('Transaction added')
}

// Generate a unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

//deleting a transaction

const handleTransactionDeleted = (id) =>{
  transactions.value = transactions.value.filter((transaction) => 
    transaction.id !== id)
  toast.success(' transaction deleted')
}
</script>
