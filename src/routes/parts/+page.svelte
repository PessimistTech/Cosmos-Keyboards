<script lang="ts">
  import { base } from '$app/paths'
  import { allVariants, PART_INFO } from '$lib/geometry/socketsParts'
  import Checkbox from '$lib/presentation/Checkbox.svelte'
  import { developer } from '$lib/store'
  import Part from './Part.svelte'
  import SharedRenderer from '$lib/3d/SharedRenderer.svelte'
  import { notNull, objEntries } from '$lib/worker/util'

  const BACK_COMPATIBLE = [
    'old-mx',
    'choc-hotswap',
    'old-mx-snap-in',
    'old-mx-hotswap',
    'old-box',
    'old-mx-snap-in-hotswap',
  ]
  const BLOCK = ['blank']

  const variantEntries = notNull(
    objEntries(PART_INFO).map(([p, info]) => ('variants' in info ? ([p, info] as const) : null))
  )
  const nonVariantEntries = notNull(
    objEntries(PART_INFO).map(([p, info]) => ('variants' in info ? null : ([p, info] as const)))
  )

  const firstEntries = nonVariantEntries.filter(
    ([part, _info]) => !BACK_COMPATIBLE.includes(part) && !BLOCK.includes(part)
  )
  const backEntries = nonVariantEntries.filter(
    ([part, _info]) => BACK_COMPATIBLE.includes(part) && !BLOCK.includes(part)
  )
</script>

<svelte:head>
  <title>Parts - Cosmos Keyboard Generator</title>
  <link rel="canonical" href="https://ryanis.cool/cosmos/parts/" />
  <link rel="icon" href="{base}/favicon.png" />
</svelte:head>

<svelte:body class="bg-slate-900 mx-4" />

<header class="block py-4 md:flex justify-between px-4">
  <a href="{base}/">
    <img
      alt="Cosmos Icon"
      src="{base}/cosmos-icon.png"
      class="v-middle inline w-12 rounded-4 mr-8 shadow-[0_2px_20px_-3px_rgba(0,0,0,0.3)] shadow-pink/50"
    />
    <h1
      class="v-middle inline my-8 capitalize text-4xl text-purple font-semibold bg-clip-text text-transparent bg-gradient-to-r from-purple-400 to-amber-600 tracking-tight"
    >
      Cosmos Keyboard Generator
    </h1>
  </a>
  <!-- svelte-ignore a11y-label-has-associated-control -->
  <label class="text-white flex items-center mr-8 <md:mt-12">
    <Checkbox basic bind:value={$developer} />
    Developer Mode
  </label>
</header>

{#if $developer}
  <div class="text-white mx-auto max-w-prose mt-6">
    The <span class="text-teal">teal regions</span> represent the boundary around the socket (where the
    adjacent walls will be located), and the
    <span class="text-red">red regions</span> represent the boundaries of the mating part (which are used
    to prevent the part from hitting the ground).
  </div>
{/if}

<main class="max-w-6xl mx-auto">
  <SharedRenderer>
    <h2>Included Parts</h2>
    <section class="parts">
      {#each firstEntries as [part, info]}
        <Part name={info.partName} {part} dev={$developer} />
      {/each}
    </section>

    {#each variantEntries as [part, info]}
      <h2>{info.partName}</h2>
      <section class="parts">
        {#each allVariants(part) as variant}
          <Part name={info.partName} {part} {variant} dev={$developer} />
        {/each}
      </section>
    {/each}

    <h2>For Backwards Compatibility</h2>
    <section class="parts">
      {#each backEntries as [part, info]}
        <Part name={info.partName} {part} dev={$developer} />
      {/each}
    </section>
  </SharedRenderer>
</main>

<style>
  h2 {
    --at-apply: 'text-teal-300 text-3xl mt-12 mx-2 mb-6';
  }
  .parts {
    --at-apply: 'grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 ';
  }
</style>
