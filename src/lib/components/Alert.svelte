<script lang='ts'>
  export let type: string
  let alertCSS = `my-2 border-2 rounded-lg py-5 px-6 mb-4 text-base mb-3`
</script>
{#if type === 'primary'}
  <div class={`bg-blue-100 text-blue-700 border-blue-700 ${alertCSS}`}>
    <slot />
  </div>
{:else if type === 'secondary'}
  <div class={`bg-purple-100 text-purple-700 border-purple-700 ${alertCSS}`}>
    <slot />
  </div>
{:else if type === 'success'}
  <div class={`bg-green-100 text-green-700 border-green-700 ${alertCSS}`}>
    <slot />
  </div>
{:else if type === 'danger'}
  <div class={`bg-red-100 text-red-700 border-red-700 ${alertCSS}`}>
    <slot />
  </div>
{:else if type === 'warning'}
  <div class={`bg-yellow-100 text-yellow-700 border-yellow-700 ${alertCSS}`}>
    <slot />
  </div>
{:else if type === 'indigo'}
  <div class={`bg-indigo-100 text-indigo-700 border-indigo-700 ${alertCSS}`}>
    <slot />
  </div>
{:else}
  <div class={`bg-gray-100 text-gray-700 border-gray-700 ${alertCSS}`}>
    <slot />
  </div>
{/if}
