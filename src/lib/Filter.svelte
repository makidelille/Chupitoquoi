<script>
    import { onMount } from "svelte";

    let { available = $bindable(), list } = $props();

    const tags = $derived(new Set(list.map((s) => s.tags).flat()));

    onMount(() => {
        tags.forEach((t) => available.push(t));
    });
</script>

{#if tags.size}
    <h4 class="flex w-full">Types de shot:</h4>

    <ul class="flex flex-row gap-2 flex-wrap w-full">
        {#each tags as t}
            <label
                for={t}
                class="p-1 bg-gray-700 text-white rounded flex flex-row gap-1 items-center justify-between w-auto capitalize relative"
            >
                <input
                    id={t}
                    name={t}
                    class="hidden"
                    type="checkbox"
                    checked={available.includes(t)}
                    onclick={() => {
                        if (available.includes(t)) {
                            available = available.filter((a) => a != t);
                        } else {
                            available.push(t);
                        }
                    }}
                />
                <span class="text-nowrap">{t}</span>
                <div class="left-1 right-1 h-0 absolute border"></div>
            </label>
        {/each}
    </ul>
{/if}

<style>
    label:has(input:checked) {
        color: black;
        background-color: var(--color-gray-100)
    }

    label:has(input:checked) div {
        display: none;
    }
    

</style>
