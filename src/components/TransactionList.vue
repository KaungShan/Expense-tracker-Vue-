<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button @click="deleteTranscation(transaction.id)" class="delete-btn">
        x
      </button>
    </li>

    <!-- <li class="minus">
        Cash <span>-$400</span><button class="delete-btn">x</button>
        </li>
        <li class="plus">
        Paycheck <span>$800</span><button class="delete-btn">x</button>
        </li> -->
  </ul>
</template>

<script setup>
import { defineProps } from "vue";

const emit = defineEmits(["transcationDeleted"]);
// const transcationList = localStorage.getItem();
// console.log(transcationList)
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
const deleteTranscation = (id) => {
  emit("transcationDeleted", id);
};
</script>

<style lang="scss" scoped></style>
