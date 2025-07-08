<script>
    import { onDestroy, onMount } from "svelte";
    import { quintInOut } from "svelte/easing";
    import Filter from "./Filter.svelte";
    import Marquee from "./Marquee.svelte";
    import Shot from "./Shot.svelte";



let {shots} = $props();

  let interval;
  const globalDuration = 3000;
  const tickRate = 1000 / 60;


  let width1 = $state();
  let widthmax = $state();
  let offsetIndex = $derived(widthmax / width1 * 0.5)


  let randomlist = $derived(
    shots.filter(s => 
    s.tags.length === 0 || s.tags.every(tag => available.includes(tag))
  ).sort(() => Math.random() - 0.5)
);


  let scrollPos  = $state(0);
  let running = $state(false);
  let done = $state(false);

  let available = $state([]);
  
  let winner = $state(undefined);



  function startAnimation() {
    running = true;
    let progress = 0;
    clearInterval(interval);
    interval = setInterval(() => {
      progress += (tickRate / globalDuration) * Math.random();
      if(progress >= 1)  {
        clearInterval(interval);
        done = true;
        let index = (scrollPos) / width1;
        winner = randomlist[~~(index + offsetIndex) % randomlist.length];
        return;
      }

      scrollPos += (1 - quintInOut(progress)) * 1000;
    }, tickRate);
  }

  onMount(() => {
    reset();
  })

  onDestroy(() => {
    clearInterval(interval)
    });
    

  function autoScroll() {
    interval = setInterval(() => {
      scrollPos += 2.5
    }, tickRate);
  }


  function reset() {
    done = false;
    running = false;
    winner = undefined;
    autoScroll();
  }

    function scrollIntoView(node) {
        node.scrollIntoView({
          behavior: "smooth"
        })
    }
</script>
  
  
  <Filter bind:available={available} list={shots}></Filter>
 
  <section class="text-white rounded relative w-full  h-48">
    <div class="absolute top-0 left-0 right-0 bottom-0 flex items-center justify-center py-4" bind:clientWidth={widthmax}>
      <Marquee bind:scrollLeft={scrollPos}  >
        {#each randomlist as s, i}
        <div class="p-1 w-42 h-full" bind:clientWidth={width1}>
          <Shot shot={s}></Shot>
        </div>
        {/each}
      </Marquee>
    </div>
    <div class="absolute h-full z-10 top-0 left-0 right-0 bottom-0 flex items-center justify-between">
      <div class="bg-gradient-to-r from-[var(--background)] to-transparent h-full w-24"></div>
      <div class="bg-red-500 w-1 h-full"></div>
      <div class="bg-gradient-to-l from-[var(--background)] to-transparent h-full w-24"></div>
    </div>
  </section>


{#if winner}
  <div class=" text-white mt-4 size-64" use:scrollIntoView>
    <Shot shot={winner}> 
    </Shot>
  </div>
     <button  onclick={reset}
      class="flex bg-white text-blue-500 py-2 px-4 rounded transition-colors duration-300 hover:bg-blue-200"
  style="font-size: 1.25rem; font-weight: bold;"> 
  J'en veux un autre&nbsp;💀 </button>
{:else if !running}
  
<button class="bg-white text-blue-500 px-4 py-2 rounded transition-colors duration-300 hover:bg-blue-200"
  style="font-size: 1.5rem; font-weight: bold;"
disabled={running} onclick={startAnimation}>
  Trouve moi un shot&nbsp;🔥
</button>

{:else}
{/if}