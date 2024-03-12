<script setup>
import { useToast } from "vue-toastification"
import { ref, computed, onMounted, onUnmounted } from "vue"
import io from "socket.io-client"

import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpenses from "./components/IncomeExpenses.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"

const toast = useToast()

const socket = io("http://localhost:3004")

onMounted(() => {
  socket.on("connect", () => {
    console.log("Connected to the backend via WebSocket")
  })

  socket.on("transactions", (updatedTransactions) => {
    transactions.value = updatedTransactions
  })
})

onUnmounted(() => {
  socket.off("connect")
  socket.off("transactions")
  socket.disconnect()
})

// onMounted(() => {
//   getTransactions()
// })

const BASE_URL = "http://localhost:3004/transactions"

// const getTransactions = async () => {
//   try {
//     const response = await fetch(BASE_URL)
//     if (!response.ok) throw new Error("Failed to fetch transactions")
//     transactions.value = await response.json()
//   } catch (error) {
//     toast.error(`Error: ${error.message}`)
//   }
// }

const addTransaction = async (transactionData) => {
  try {
    const response = await fetch(BASE_URL, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        description: transactionData.description,
        amount: transactionData.amount,
      }),
    })
    if (!response.ok) throw new Error("Failed to add transaction")
    const newTransaction = await response.json()
    transactions.value.push(newTransaction)
    toast.success("Transaction added")
  } catch (error) {
    toast.error(`Error: ${error.message}`)
  }
}

const updateTransaction = async (transaction) => {
  try {
    const response = await fetch(`${BASE_URL}/${transaction.id}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        description: transaction.description,
        amount: transaction.amount,
      }),
    })
    if (!response.ok) throw new Error("Failed to update transaction")
    const updatedTransaction = await response.json()

    const index = transactions.value.findIndex((t) => t.id === updatedTransaction.id)
    if (index !== -1) {
      transactions.value.splice(index, 1, updatedTransaction)
    }
    toast.success("Transaction updated successfully")
  } catch (error) {
    toast.error(`Error: ${error.message}`)
  }
}

const deleteTransaction = async (id) => {
  try {
    const response = await fetch(`${BASE_URL}/${id}`, {
      method: "DELETE",
    })
    if (!response.ok) throw new Error("Failed to delete transaction")
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
    toast.success("Transaction deleted")
  } catch (error) {
    toast.error(`Error: ${error.message}`)
  }
}

const transactions = ref([])

// get total amount of expenses
const total = computed(() => transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0))

// get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

// get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="deleteTransaction" @transactionUpdated="updateTransaction" />
    <AddTransaction @transactionSubmitted="addTransaction" /><!--  here goes the name of the event you defined inside -->
  </div>
</template>
