<script>
  import { setContext, onMount, afterUpdate } from "svelte";

  //components
  // import Github from "./Github.svelte";
  import GithubAwait from "./Github.svelte";

  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";
  //data
  //import expensesData from "./expenses";

  // variables
  let expenses = [];
  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  // toggle form variables
  let isFormOpen = false;
  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);
  // functions
  function showForm() {
    isFormOpen = true;
  }
  function hideForm() {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = null;
  }
  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }
  function clearExpenses() {
    expenses = [];
  }
  function handleSubmit() {
    if (isEditing) {
      editExpense();
    } else {
      addExpense();
    }
    setId = null;
    setAmount = null;
    setName = "";
    hideForm();
  }

  function addExpense() {
    let expense = { id: Math.random() * Date.now(), name: setName, amount: setAmount };
    expenses = [expense, ...expenses];
  }
  function setModifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }
  function editExpense() {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name: setName, amount: setAmount } : { ...item };
    });
  }
  // context
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);
  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem("expenses") ? JSON.parse(localStorage.getItem("expenses")) : [];
  });
  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<!-- <style></style> -->
<!-- <CSS/STYLING -->

<Navbar {showForm} />
<!-- <main>
  <GithubAwait />
</main> -->
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm bind:name={setName} bind:amount={setAmount} {isEditing} {handleSubmit} {hideForm} />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}> clear expenses </button>
</main>
