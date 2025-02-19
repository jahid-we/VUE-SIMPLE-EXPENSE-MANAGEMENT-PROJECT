<script setup>
import { defineProps, defineModel, computed } from "vue";

const props = defineProps(["lists"]); // Receive transaction list as a prop
const filterType = defineModel(); // Two-way binding for filter selection

// Compute filtered transactions based on selected type
const filteredTransactions = computed(() => {
  if (filterType.value === "income") {
    return props.lists.filter((item) => item.type === "Income");
  } else if (filterType.value === "expense") {
    return props.lists.filter((item) => item.type === "Expense");
  }
  return props.lists; // Show all transactions by default
});

// Function to delete a transaction and update local storage
const deleteTransaction = (index) => {
  props.lists.splice(index, 1);
  localStorage.setItem("transactions", JSON.stringify(props.lists));
};
</script>

<template>
  <div class="container mt-4">
    <h3 class="text-center fw-bold">Transaction List</h3>

    <!-- Filter Radio Buttons -->
    <div class="mb-3 d-flex justify-content-center gap-3">
      <div class="form-check">
        <input class="form-check-input" type="radio" id="all" value="all" v-model="filterType" />
        <label class="form-check-label" for="all">All</label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="radio" id="income" value="income" v-model="filterType" />
        <label class="form-check-label text-success" for="income">Income</label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="radio" id="expense" value="expense" v-model="filterType" />
        <label class="form-check-label text-danger" for="expense">Expense</label>
      </div>
    </div>

    <!-- Responsive Table -->
    <div class="table-responsive shadow-sm rounded">
      <table class="table table-striped table-hover">
        <thead class="table-dark text-center">
          <tr>
            <th>Title</th>
            <th>Amount</th>
            <th>Type</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody class="text-center">
          <tr v-for="(item, index) in filteredTransactions" :key="item.id">
            <td>{{ item.title.toUpperCase() }}</td>
            <td class="fw-bold">${{ item.amount }}</td>
            <td>
              <span :class="item.type === 'Income' ? 'badge bg-success' : 'badge bg-danger'">
                {{ item.type }}
              </span>
            </td>
            <td>
              <button class="btn btn-outline-danger btn-sm" @click="deleteTransaction(index)">
                <i class="bi bi-trash"></i> Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: auto;
}

.table-responsive {
  background: white;
  padding: 15px;
  border-radius: 10px;
}

.table th, .table td {
  vertical-align: middle;
}
</style>
