<script setup>
import { ref, watch } from "vue";

const transactionList = defineModel(); // Two-way binding for transactions
const addButton = ref(false); // Controls visibility of the add form

// Reactive object for new transaction input
const addNewTransactionList = ref({
  id: "",
  title: "",
  amount: "",
  type: "",
});

// Watch for changes in the transaction list and update local storage
watch(
  transactionList,
  (newVal) => {
    if (newVal) {
      localStorage.setItem("transactions", JSON.stringify(newVal));
    }
  },
  { deep: true }
);

// Add new transaction
const addTransaction = () => {
  addNewTransactionList.value.id = transactionList.value.length + 1;
  transactionList.value.push({ ...addNewTransactionList.value });

  // Reset input fields
  addNewTransactionList.value = { id: "", title: "", amount: "", type: "" };

  addButton.value = false; // Hide form after adding transaction
};
</script>

<template>
  <div class="container mt-3">
    <!-- Add Transaction Button -->
    <h3 v-show="addButton" class="text-center">Add New Transaction</h3>
    <div class="text-center">
      <button
        v-show="!addButton"
        @click="addButton = true"
        class="btn btn-primary rounded-pill px-4 py-2"
      >
        Add New Transaction
      </button>
    </div>

    <!-- Add Transaction Form -->
    <div v-show="addButton" class="mt-3">
      <form
        @submit.prevent="addTransaction"
        class="row g-2 p-3 bg-light shadow-sm rounded"
      >
        <div class="col-md-4">
          <input
            type="text"
            class="form-control rounded-pill"
            placeholder="Title"
            v-model="addNewTransactionList.title"
            required
          />
        </div>
        <div class="col-md-4">
          <input
            type="number"
            class="form-control rounded-pill"
            placeholder="Amount"
            v-model="addNewTransactionList.amount"
            required
          />
        </div>
        <div class="col-md-4">
          <select
            class="form-select rounded-pill"
            v-model="addNewTransactionList.type"
            required
          >
            <option value="">Type</option>
            <option value="Income">Income</option>
            <option value="Expense">Expense</option>
          </select>
        </div>
        <div class="col-12 text-center">
          <button type="submit" class="btn btn-success rounded-pill px-4 py-2">
            Add
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
}
</style>
