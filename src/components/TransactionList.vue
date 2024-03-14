<script setup>
import { defineProps, ref, reactive } from "vue"
const emit = defineEmits(["transactionDeleted", "transactionUpdated"])

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
})
const editingId = ref(null)
const tempTransaction = reactive({ description: "", amount: 0 })

const startEditing = (transaction) => {
  editingId.value = transaction.id
  tempTransaction.description = transaction.description
  tempTransaction.amount = transaction.amount
}

const deleteTransaction = (id) => {
  emit("transactionDeleted", id)
}

const updateTransaction = () => {
  if (tempTransaction.description.trim() !== "" && tempTransaction.amount !== 0) {
    emit("transactionUpdated", { id: editingId.value, ...tempTransaction })
    editingId.value = null // stop edit mode
  }
}

const checkEnterKey = (event) => {
  if (event.key === "Enter") {
    updateTransaction()
  }
}
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount > 0 ? 'plus' : 'minus'">
      <template v-if="editingId === transaction.id">
        <!-- add cancel mode -->
        <input v-model="tempTransaction.description" @keyup.enter="checkEnterKey" class="edit-input" />
        <input v-model="tempTransaction.amount" type="number" @keyup.enter="checkEnterKey" class="edit-input" />
      </template>
      <template v-else>
        {{ transaction.description }} <span>${{ transaction.amount }}</span>
      </template>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
      <button class="edit-btn" @click="startEditing(transaction)">&#9998;</button>
      <!-- Cambiado aquí -->
    </li>
  </ul>
</template>
