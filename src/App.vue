<script setup>
import { ref, computed} from "vue";

const now = new Date();

const formattedDateTime = now.toLocaleString("en-US", {
  month: "numeric",
  day: "numeric",
  year: "numeric",
  hour: "numeric",
  minute: "2-digit",
  hour12: true
});



const expenses = ref([]);
const newExpenseName = ref("");
const newExpenseAmount = ref(null);
const newExpenseCategory = ref("");
const monthsIncome = ref(1253.00);
const today = new Date();
const monthName = today.toLocaleString("default", { month: "long" });
const fName = "John";

const addExpense = () => {
  if (newExpenseName.value == "" || newExpenseAmount.value == 0) {
    alert("ADD EXPENSE NAME OR AMOUNT");
    return;
  }

  expenses.value.push({
    name: newExpenseName.value,
    amount: newExpenseAmount.value,
    category: newExpenseCategory.value,
    today: new Date()
  });

  newExpenseAmount.value = null;
  newExpenseName.value = "";
  newExpenseCategory.value = "";
};

const formatCurrency = (v) => v.toFixed(2);


const deleteExpense = (index) => {
  expenses.value.splice(index, 1);
};

const totalSpent = computed(() => {
  return expenses.value.reduce((sum, exps) => sum + exps.amount, 0);
});

const left = computed(() => monthsIncome.value - totalSpent.value);

</script>

<template>
  <div class="page">
    <div class="headings">
      <div class="upperLeft">
        <p class="welcome">Welcome back, {{ fName }}!</p>
        <h1>Expenses for {{ monthName }}</h1>
      </div>
      <div class="monthlyBud">
        {{ monthName }} Income: ${{ monthsIncome }}
      </div>
    </div>
    <form class="content" @submit.prevent="addExpense">
      <input class="expense" v-model="newExpenseName" placeholder="Expense: " />
      <input class="amount" type="number" v-model="newExpenseAmount" placeholder="Amount: " />
      <select v-model="newExpenseCategory">
        <option value="">Category:</option>
        <option value="Food">Food</option>
        <option value="Entertainment">Entertainment</option>
        <option value="Utilities">Utilities</option>
        <option value="Rent">Rent</option>
        <option value="Bills">Bills</option>
        <option value="Groceries">Groceries</option>
      </select>
      <button class="add" type="button" @click="addExpense()">Add</button>
    </form>
    <ul class="expense-list">
  <li v-for="(expense, index) in expenses" :key="index"class="expense-row">
      <span class="expense-name">{{ expense.name }}</span>
      <span class="expense-td">
        {{ formattedDateTime  }}
      </span>
      <span class="expense-details">
        ${{ expense.amount }} 
      </span>
      <span class="expense-cat"> {{ expense.category }}</span>

    <button class="delete-btn" @click="deleteExpense(index)">
      x
    </button>
  </li>
</ul>

        <div>
          <h2 class="totals">Total for this month: ${{ formatCurrency(totalSpent) }}</h2>
          <h3 class="totals"> Remaining budget left: $ {{ formatCurrency(left) }} </h3>
      </div>
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+3:ital,wght@0,200..900;1,200..900&display=swap");

* {
  /*outline: 1px solid red; */
  font-family: "Source Sans 3";
  font-weight: 400;
}

.add{
  margin-left: 10px;
  color: lightgrey;
  border-radius: 0;
}

button :hover {
  background-color: blue;
}

.delete-btn{
  text-align: right;
  align-self: center;
  justify-self: end;
}

.monthlyBud{
  text-align: right;
  align-self: end;
  justify-self: end;
  padding-bottom: 13%;
}

.content{
  width: 100%;
  margin-left: 0;
}

.amount{
  width: 25%;

}

.expense-td{
  font-style: italic;
  color: #6b7280;
  font-size: 0.85rem;

}

.page {
  margin: 0;
  padding: 0;
  width: 90vw;
  height: auto;
  overflow: hidden;
}

.welcome {
  font-size: medium;
  text-align: left;
  margin: 0 auto;
}

.headings {
  display: grid;
  grid-template-columns: 50fr 50fr;
  grid-template-rows: 1fr;
  grid-column-gap: 1px;
}

.upperLeft {
  grid-column-start: 1;
  grid-row-start: 1;
}

.totals {
  text-align: right;
  margin: 0 auto;
  font-size: large;
  font-weight: 400;
}

.expense-list{
  margin-top: 30px;
  padding: 0;
  max-width: 100vw;
}
.expense-row{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  width: 100%;
  align-items: center;
  border-bottom: 1px solid #e5e7eb;
}

.expense-name{
  justify-self: start;
  padding-left: 10px;
  font-weight: 500;
  
}

.expense-details{
  justify-self: center;

}

ul, li {
  list-style-type: none ;
}

h1 {
  font-size: xx-large;
  font-weight: 400;
  margin: 0 auto;
  text-align: left;
  padding-bottom: 12%;
}

input {
  margin: 10px;
  border: 0.5px;
  width: 25%;
  background-color: darkgray;
}

::placeholder {
  color: lightgrey;
}

select {
  background: darkgrey;
  color: lightgrey;
  width: 25%;
}

form {
  display: flex;
  gap: 10px;
  align-items: center;
  width: 100%;
}

ul {
  justify-items: left;
}

.add {
}
</style>
