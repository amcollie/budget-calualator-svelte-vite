<script>
  export let showForm
  export let isFormOpen
</script>
<nav class="nav">
  <div class="nav__center">
    <h1 class="nav__title">budget calculator</h1>
    <button 
      type="button"
      class="{isFormOpen? 'far fa-minus-square' : 'far fa-plus-square'} nav__btn"
      aria-label="Add Item"
      on:click={showForm}
    >
      add item
    </button>
  </div>
</nav>

<style>
  .nav {
    background: var(--primaryColor);
    height: 5rem;
    display: flex;
    align-items: center;
    box-shadow: var(--lightShadow);
  }

  .nav__center {
    width: 85%;
    max-width: 35rem;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
  }

  .nav__title {
    text-transform: capitalize;
    color: var(--mainWhite);
    margin-bottom: 0;
    letter-spacing: 2px;
  }

  .nav__btn {
    background: transparent;
    font-size: 1.3rem;
    border: none;
    cursor: pointer;
    text-transform: capitalize;
    transition: var(--mainTransition);
  }

  .nav__btn:hover {
    color: var(--darkGrey);
  }
</style>