<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transcationDeleted="handleTranscationDeleted"
    />
    <AddTransaction @transcationSubmitted="handleTranscationSubmitted" />
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import AddTransaction from "./components/AddTransaction.vue";
import TransactionList from "./components/TransactionList.vue";
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const saveTranscations = JSON.parse(localStorage.getItem("transcations"));
  if (saveTranscations) {
    transactions.value = saveTranscations;
  }
});
//get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//add transcation
const handleTranscationSubmitted = (transactionData) => {
  transactions.value.push({
    id: Math.random() * (1000).toString(),
    text: transactionData.text,
    amount: parseFloat(transactionData.amount),
  }),
    saveTranscationsToLocalStorage();

  toast.success("Transaction added!");
};

//delete transcation
const handleTranscationDeleted = (id) => {
  (transactions.value = transactions.value.filter((transcation) => {
    return transcation.id !== id;
  })),
    saveTranscationsToLocalStorage();

  toast.success("Trenscation removed");
};

//save to localstorage
const saveTranscationsToLocalStorage = () => {
  localStorage.setItem("transcation", JSON.stringify(transactions.value));
};
</script>
<style></style>
