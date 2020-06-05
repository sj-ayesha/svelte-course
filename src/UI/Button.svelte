<script>
  export let type;
  export let caption;
  export let href;
</script>

<style>

</style>

{#if href}
  <a {href}>{caption}</a>
{:else}

  <button {type}>{caption}</button>
{/if}
