<script>
  export let expenses = []
  export let clearExpenses

  import { flip } from 'svelte/animate'
  import { fly } from 'svelte/transition'

  import SectionTitle from './Title.svelte'
  import Expense from './Expense.svelte'
</script>

<section>
  <SectionTitle title="expense list" />
  <ul> 
    {#each expenses as expense, index (expense.id)}
      <li 
        in:fly={{
          x: 200, 
          delay:index*500
        }}
        out:fly={{
          x: -200
        }}
        animate:flip
      >
        <Expense {...expense} />
      </li>
    {:else}
      <h2>no expenses added to the list</h2>
    {/each}
  </ul>
  <button 
    class="btn btn-primary btn-block"
    type="button"
    on:click="{clearExpenses}"
  >
    clear expenses
  </button>
</section>


<style>
  h2 {
    text-transform: capitalize;
  }
  li {
    list-style: none;
  }
</style>