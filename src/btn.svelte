<script lang="typescript">

  export let handle: () => void
  export let title: string
  export let disable = false
  
</script>

<span class="btn" on:click={handle} class:disabled={disable}>
  {title}
</span>

<style lang="scss">
  .btn {
    padding: .8em 1em;
    margin: 1em;
    border: none;
    box-shadow: 3px 3px 10px silver;
    border-radius: 8px;
    cursor: pointer;
  }
  .disabled {
    background-color: rgb(192, 192, 192);
    cursor: default;
  }
</style>