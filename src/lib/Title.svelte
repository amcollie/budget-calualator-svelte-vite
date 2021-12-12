<script>
  export let title = "default title"
  $: uppercaseTitle = title.toUpperCase()
</script>

<div class="main__title">
  <h2>{uppercaseTitle}</h2>
</div>

<style type="text/css">
  :global(.main__title) {
    text-transform: capitalize;
    color: var(--primaryColor);
    margin-bottom: 2rem;
  }
</style>