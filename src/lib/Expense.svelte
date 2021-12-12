<script>
  export let id
  export let name = ''
  export let amount = 0.0

  import { getContext } from 'svelte'
  import { slide } from 'svelte/transition'

  let displayAmount = false

  function toggleAmount() {
    displayAmount = !displayAmount
  }

  const removeExpense = getContext('removeExpense')
  const setModifiedExpense = getContext('modifyExpense')
</script>

<article class="single__expense">
  <div class="expense__info">
    <h2>{name} <button class="{displayAmount ? 'fas fa-caret-up' : 'fas fa-caret-down'} amount__btn" on:click="{() => displayAmount = !displayAmount}"></button></h2>
    {#if displayAmount}
      <h4
        transition:slide
      >
        amount: ${amount}
      </h4>
    {/if}
  </div>
  <div class="expense__buttons">
    <button
      class="fas fa-pen expense__btn edit__btn" 
      aria-label="Edit"
      on:click={() => setModifiedExpense(id)}
    >
    </button>
    <button 
      class="fas fa-trash expense__btn delete__btn"
      on:click="{() => removeExpense(id)}"
      aria-label="Delete"
    >
    </button>
  </div>
</article>

<style>
  .single__expense {
    display: flex;
    justify-content: space-between;
    text-transform: capitalize;
    align-items: center;
    margin-bottom: 2rem;
    padding: 1.3rem 1rem;
    border-radius: var(--mainBorderRadius);
    background: var(--mainWhite);
    box-shadow: var(--lightShadow);
  }

  .single__expense h2 {
    margin-bottom: 0;
    font-weight: 300;
  }

  .single__expense h4 {
    margin-bottom: 0;
    margin-top: 1rem;
    font-weight: 300;
    color: var(--primaryColor);
  }

  .expense__btn {
    font-size: 1rem;
    background: transparent;
    border: none;
    margin: 0 0.4rem;
    transition: var(--mainTransition);
    cursor: pointer;
  }

  .expense__btn:hover {
    transform: scale(1.2);
  }

  .amount__btn {
    font-size: 1.3rem;
    background: transparent;
    border: none;
    cursor: pointer;
    color: var(--primaryColor);
  }

  .delete__btn {
    color: red;
  }

  .edit__btn {
    color: green;
  }
</style>