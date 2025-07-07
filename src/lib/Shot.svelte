<script>
    let {shot, children = undefined} = $props();
</script>

<div
    class="bg-gray-800 gap-2 flex flex-col space text-white p-4 rounded-lg shadow-lg h-full justify-between align-middle items-center">
    <h2 class="text-bold text-left capitalize">{shot.name}</h2>
    {#if shot.image}
    <div class="flex overflow-hidden">
        <img src={shot.image} alt={shot.name}/>
    </div>
    {:else}
    <span></span>
    {/if}
    <div class="flex flex-wrap gap-2 mt-2 justify-start w-full">
        {#each shot.tags as tag}
            <span class="bg-gray-100 p-1 text-black rounded text-xs">{tag}</span>
        {/each}
    </div>
</div>

