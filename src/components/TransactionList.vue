<script setup>
import { defineProps } from "vue"
const emit = defineEmits(["transactionDeleted"])

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
})

const deleteTransaction = (id) => {
  emit("transactionDeleted", id)
}

const updateTransaction = (id) => {}
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount > 0 ? 'plus' : 'minus'">
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
      <button class="edit-btn" @click="updateTransaction(transaction.id)">E</button>
    </li>
  </ul>
</template>
