<script>
import Tile from "./Tile.svelte";
export let row;
export let rowNumber;
export let onClickCallback;
export let disabled;
export let highlightedTiles;
</script>

<div
  class='row'
>
  {#each row as value, index}
    <Tile
      {value}
      coordinate={[rowNumber, index]}
      {onClickCallback}
      disabled={disabled}
      isHighlighted={highlightedTiles[index]}
    />

    {#if index < row.length-1}
      <div
        class='vertical-line'
      />
    {/if}
  {/each}
</div>

<style>
  .vertical-line {
    border-left: 0.2rem solid white;
  }

  .row {
    display: flex;
    flex: 1;
  }
</style>