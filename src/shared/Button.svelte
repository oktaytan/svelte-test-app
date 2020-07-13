<script>
  export let type = "primary";
  export let flat = false;
  export let inverse = false;
</script>

<style>
  button {
    border: 0;
    cursor: pointer;
    border-radius: 6px;
    padding: 8px 12px;
    font-weight: bold;
    box-shadow: 1px 2px 4px rgba(0, 0, 0, 0.2);
  }

  .primary {
    background-color: crimson;
    color: #fff;
  }

  .secondary {
    background-color: #12ca7e;
    color: #fff;
  }

  .primary.inverse {
    color: crimson;
    background-color: #fff;
    border: 2px solid crimson;
  }

  .secondary.inverse {
    color: #12ca7e;
    background-color: #fff;
    border: 2px solid #12ca7e;
  }

  .flat {
    box-shadow: none;
  }
</style>

<button class={type} class:flat class:inverse on:click>
  <slot />
</button>
