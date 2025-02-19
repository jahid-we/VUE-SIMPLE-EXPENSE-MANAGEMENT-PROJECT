<script setup>
import { ref, onMounted } from 'vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

const filterType = ref('all');
const transactionList = ref([]);

onMounted(() => {
  const storedTransactions = JSON.parse(localStorage.getItem("transactions")) || [];
  transactionList.value = storedTransactions;
});
</script>

<template>
  <div class="container py-4">
    <div class="row justify-content-center">
      <div class="col-lg-6 col-md-8 col-sm-12">
        <add-transaction v-model="transactionList" />
      </div>
    </div>

    <div class="row justify-content-center mt-4">
      <div class="col-lg-8 col-md-10 col-sm-12">
        <transaction-list v-model="filterType" :lists="transactionList" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 100%;
}
</style>

