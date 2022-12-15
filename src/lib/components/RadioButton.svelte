<script lang='ts'>
  type Option = {
    label: string,
    value: any
  }
  export let question: string
  export let options: Option[]
  export let userSelected: string | boolean

  const uniqueID = Math.floor(Math.random() * 100000)
  const sluggify = (str = '') => str.toLowerCase().replace(/ /g, '-').replace(/\./g, '')
</script>

<div>
  <div class="font-bold">{question}</div>
  {#each options as { value, label }}
    <div>
      <input 
        class="cursor-pointer"
        type="radio"
        id={`${sluggify(label)}-${uniqueID}`}
        bind:group={userSelected}
        value={value}
      >
      <label
        class="cursor-pointer"
        for={`${sluggify(label)}-${uniqueID}`}>
        {label}
      </label>
    </div>
  {/each}
</div>