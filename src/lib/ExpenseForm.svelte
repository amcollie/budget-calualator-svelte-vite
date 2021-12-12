<script>
  export let name = ''
  export let amount = null
  export let formMode
  export let handleSubmit
  export let closeForm

  import Title from './Title.svelte'

  $: isEmpty = !name || !amount
</script>

<section class="form">
  <Title title='{formMode} expense' />
  <form 
    class='expense-form' 
    on:submit|preventDefault={handleSubmit}
  >
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="text" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">please fill out all form fields</p>
    {/if}
    <button 
      type="submit" 
      class="btn btn-block {isEmpty ? 'disabled' : ''}" 
      disabled={isEmpty}
    >
      {formMode} expense
    </button>
    <button 
      type="button" 
      class="fas fa-times close-btn" 
      on:click={closeForm}
    >
      close
    </button>
  </form>
</section>

<style>
  .expense-form {
    background: var(--mainWhite);
    padding: 1.25rem 1rem;
    text-transform: capitalize;
    border-radius: var(--mainBorderRadius);
    box-shadow: var(--lightShadow);
  }

  .form-control label {
    display: block;
  }
  .form-control input {
    width: 100%;
    border: none;
    border-bottom: 2px solid var(--darkGrey);
    margin-bottom: 1.25rem;
    padding: 0.5rem;
    font-size: 1.2rem;
  }
  .form-empty {
    text-align: center;
    color: red;
  }
  .form {
    position: relative;
    margin-bottom: 5rem;
  }
  .close-btn {
    position: absolute;
    top: 0;
    right: 0;
    font-size: 1.2rem;
    text-transform: capitalize;
    display: flex;
    align-items: center;
    width: 4.2rem;
    justify-content: space-between;
    background: transparent;
    border: none;
    color: red;
    cursor: pointer;
  }
</style>