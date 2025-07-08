<script>
  import menus from "./data";
  import Roulette from "./lib/Roulette.svelte";


  let options =  Object.keys(menus);

  let list = $state(options[0]);

</script>


<header class="flex flex-col gap-2 w-full justify-center items-center p-4">
  <h1 class="text-3xl flex w-full">Le Chupitoquoi</h1>
  <h6 class="text-xl flex w-full">Tu ne sais pas quoi choisir ? Et bien ce n'est pas grave.
     Le chupitoquoi est là pour t'aider à trouver le shot qu'il te faut.
     Tu peux exclure certain type de shooter si tu le souhaites. Mais le mieux reste de tout avoir
  </h6>
</header>

<main class="flex flex-col w-auto justify-start items-center gap-4 p-12">

  <div class="flex flex-row w-full align-baseline gap-0.5">
    <label for="menu">Carte</label>
    <select name="menu" bind:value={list}>
      {#each options as opt}
        <option value={opt}>{opt}</option>
      {/each}
    </select>
  </div>


  {#if list}
  {#key list}
    <Roulette shots={menus[list]}></Roulette>
  {/key}
  {/if}

</main>

<div style="flex: 1 1 auto; display: flex;"></div>
<footer class="bottom-0 left-0 right-0 flex flex-col items-center justify-center py-1 gap-1">
  <div>
    Ce site n'est pas affilié avec l'Esprit Chupitos
  </div>
  <a href="https://github.com/makidelille/Chupitoquoi" target="_blank">Voir la source sur github</a>
</footer>

<style>
</style>
