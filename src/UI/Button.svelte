<script>
  export let type = "button";
  export let caption;
  export let href = null;
</script>

<style>

</style>

{#if href}
  <a {href}>{caption}</a>
{:else}
  <button {type}>{caption}</button>
{/if}
