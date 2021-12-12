<script>
  import { setContext, afterUpdate, onMount } from 'svelte'

  import { v4 as uuidv4 } from 'uuid'

  import Navbar from './lib/Navbar.svelte'
  import Title from './lib/Title.svelte'
  import ExpenseList from './lib/ExpenseList.svelte'
  import Totals from './lib/Totals.svelte'
  import ExpenseForm from './lib/ExpenseForm.svelte'
  import Modal from './lib/Modal.svelte'

  let expenses = []

  let setId = null
  let setName = ''
  let setAmount = null

  let isFormOpen = false

  $: formMode = setId ? 'edit' : 'add'
  $: total = expenses.reduce((tot, exp) => tot + exp.amount, 0)

  function addExpense() {
    const expense = {
      id: uuidv4(),
      name: setName,
      amount: parseFloat(setAmount)
    }
    expenses = [...expenses, expense]
  }

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id)
  }

  function clearExpenses() {
    expenses = []
  }

  function setModifiedExpense(id) {
    const expense = expenses.find(item => item.id === id)

    setId = expense.id
    setName = expense.name
    setAmount = expense.amount
    showForm()
  }

  function editExpense() {
    expenses = expenses.map(expense => {
      return expense.id === setId ? { ...expense, name: setName, amount: parseFloat(setAmount) } : {...expense}
    })
    setLocalStorage()
  }

  function handleSubmit() {
    if (formMode === 'edit') {
      editExpense()
    } else {
      addExpense()
    }
    
    setId = null
    setName = ''
    setAmount = null

    closeForm()
  }

  function showForm() {
    isFormOpen = true
  }

  function closeForm() {
    isFormOpen = false

    setId = null
    setName = ''
    setAmount = null
  }

  setContext('removeExpense', removeExpense)
  setContext('modifyExpense', setModifiedExpense)

  function setLocalStorage() {
    localStorage.setItem('expenses', JSON.stringify(expenses))
  }

  onMount(() => {
    expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) : []
  })

  afterUpdate(() => setLocalStorage())
</script>

<header>
  <Navbar {showForm} {isFormOpen} />
</header>

<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm 
        {formMode} 
        {handleSubmit}
        {closeForm}
        bind:name={setName} 
        bind:amount={setAmount} 
      />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpenseList expenses={expenses} {clearExpenses} />
</main>

<style>
  @import url("https://fonts.googleapis.com/css?family=Catamaran:300,400&display=swap");

  :global(:root) {
    --primaryColor: #2892d7;
    --mainWhite: #fff;
    --offWhite: #f7f7f7;
    --mainBlack: #222;
    --mainGrey: #ececec;
    --darkGrey: #afafaf;
    --mainTransition: all 0.3s linear;
    --mainSpacing: 0.3rem;
    --lightShadow: 2px 5px 3px 0px rgba(0, 0, 0, 0.5);
    --darkShadow: 4px 10px 5px 0px rgba(0, 0, 0, 0.5);
    --mainBorderRadius: 0.25rem;
  }

  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :global(body) {
    font-family: "Catamaran", sans-serif;
    color: var(--mainBlack);
    background: var(--mainGrey);
    line-height: 1.4;
    font-size: 1rem;
    font-weight: 300;
  }

  :global(
    h1,
    h2,
    h3,
    h4,
    h5,
    h6
  ) {
    font-family: "Catamaran", sans-serif;
    margin-bottom: 1.25rem;
    letter-spacing: var(--mainSpacing);
  }

  :global(p) {
    margin-bottom: 1.25rem;
  }

  :global(ul) {
    list-style-type: none;
  }

  :global(a) {
    text-decoration: none;
    color: var(--mainBlack);
  }

  :global(
    .btn,
    .btn-white,
    .btn-primary
  ) {
    text-transform: uppercase;
    letter-spacing: var(--mainSpacing);
    color: var(--primaryColor);
    border: 2px solid var(--primaryColor);
    padding: 0.45rem 0.8rem;
    display: inline-block;
    transition: var(--mainTransition);
    cursor: pointer;
    font-size: 0.8rem;
    border-radius: var(--mainBorderRadius);
    background: transparent;
  }
  :global(.btn:hover) {
    background: var(--primaryColor);
    color: var(--mainWhite);
  }
  :global(.btn-white) {
    background: transparent;
    color: var(--primaryColor);
    border-color: var(--primaryColor);
  }
  :global(.btn-white:hover) {
    background: var(--mainWhite);
    color: var(--primaryColor);
  }
  :global(.btn-primary) {
    background: var(--primaryColor);
    color: var(--mainWhite);
    border-color: var(--primaryColor);
  }
  :global(.btn-primary:hover) {
    background: transparent;
    color: var(--primaryColor);
  }
  :global(.btn-block) {
    width: 100%;
    display: block;
    margin: 0 auto;
    box-shadow: var(--lightShadow);
  }
  :global(.disabled) {
    color: var(--darkGrey);
    border-color: var(--darkGrey);
  }
  :global(.disabled:hover) {
    background: transparent;
    color: var(--darkGrey);
    border-color: var(--darkGrey);
  }

  .content {
    padding: 3rem 0;
    width: 85vw;
    max-width: 35rem;
    margin: 0 auto;
  }
</style>
