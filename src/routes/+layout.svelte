<script lang='ts'>
  export const prerender = true
  import '../app.css'
</script>

<div class="flex justify-center">
  <div class="m-20 max-w-fit bg-white rounded-xl">
    <slot />
  </div>
</div>

<div class="text-center">
  <a class="text-sm text-gray-700" href="https://squid.software" target="_blank" rel="noreferrer">
    🦑
    <span class="underline underline-offset-4">Made by Squid</span>
  </a>
</div>
