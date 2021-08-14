<script>
  export let value;
  export let coordinate;
  export let onClickCallback;
  export let disabled;
  export let isHighlighted;

  const handleClick = () => {
    onClickCallback(coordinate);
  }
</script>

{#if value === 0}
  <button
    class={isHighlighted ? 'highlighted' : ''}
    {disabled}
    on:click={handleClick}
  />
{:else if value === 1}
  <button
    class={`x ${isHighlighted ? 'highlighted' : ''}`}
    disabled
  >
    X
  </button>
{:else}
  <button
    class={`o ${isHighlighted ? 'highlighted' : ''}`}
    disabled
  >
    O
  </button>
{/if}

<style>
  button {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0;
    padding: 0;
    background: transparent;
    border: none;
    color: red;
    font-weight: bold;
    font-size: 4rem;
  }

  .highlighted {
    background-color: #557180;
  }

  .o {
    color: #64dd17
  }

  .x {
    color: #bf360c;
  }
</style>