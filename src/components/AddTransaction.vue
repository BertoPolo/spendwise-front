<script setup>
import { ref } from "vue"
import { useToast } from "vue-toastification"

const description = ref("")
const amount = ref("")
const toast = useToast()

const emit = defineEmits(["transactionSubmitted"])

const onSubmit = () => {
  if (!description.value || !amount.value) {
    toast.error("Both fields must be filled")
    return
  }

  const transactionData = {
    description: description.value,
    amount: parseFloat(amount.value),
  }
  emit("transactionSubmitted", transactionData)

  description.value = ""
  amount.value = ""
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="from" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="description">Description</label>
      <input type="text" v-model="description" id="description" placeholder="Enter description..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount<br />(negative = expense, positive = income)</label>
      <input type="number" v-model="amount" id="amount" placeholder="Enter amount.." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
