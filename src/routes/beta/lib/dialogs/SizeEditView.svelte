<script lang="ts">
  import DecimalInput from '../editor/DecimalInput.svelte'
  import { createEventDispatcher } from 'svelte'

  let nRows: number = 4
  let nCols: number = 5

  const dispatch = createEventDispatcher()

  function changeSize() {
    dispatch('size', [nRows, nCols])
  }
</script>

<ul class="my-4 text-center">
  <DecimalInput divisor={1} small bind:value={nRows} />
  <span class="ml-2">rows <span class="ml-1">&times;</span></span>
  <DecimalInput divisor={1} small bind:value={nCols} />
  <span class="ml-2">columns</span>
  <div class="mb-4" />
  <button class="button mx-auto" on:click={() => changeSize()}>Set</button>
</ul>

<style>
  .button {
    --at-apply: 'bg-slate-300 dark:bg-gray-900  hover:bg-teal-500 dark:hover:bg-pink-700 dark:text-white font-bold py-2 px-4 rounded focus:outline-none border border-transparent focus:border-pink-500 flex items-center gap-2';
  }
</style>
