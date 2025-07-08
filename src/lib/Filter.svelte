
<script>
    import {  onMount } from "svelte";

    
    let {
        available = $bindable(),
        list
    } = $props();

    const tags = $derived(new Set(list.map(s => s.tags).flat()));


    onMount(() => {
        tags.forEach(t => available.push(t));
    })

    

    $effect(() => {
        console.log(available); 
    })


</script>

{#if tags.size}

<h4 class="flex w-full">
    Types de shot:
</h4>

<ul class="flex flex-row gap-2 flex-wrap w-full">
    {#each tags as t}
    <label for={t} class="p-1 bg-gray-100 text-black rounded flex flex-row gap-1 items-center justify-between w-auto capitalize">
        <input id={t} name={t} type="checkbox" checked={available.includes(t)}
         onclick={
            () => {
                if(available.includes(t)) {
                    available = available.filter(a => a != t);
                } else {
                    available.push(t);
                }
            }
        }/>
        <span class="text-nowrap">{t}</span>
    </label>
    
    {/each}
</ul>

{/if}