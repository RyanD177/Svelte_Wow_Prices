<script>
  import { onMount } from "svelte";
  import {

    getItem,
    getToken,
    AuthToken,
    item,
  } from "../../getToken.svelte";
  import { page } from "$app/stores";


  const slug = $page.params;
  let filteredAuctions = [];
  let foundItem = {};

  onMount(async () => {
    getToken().then(() => {
      AuthToken.subscribe((value) => {
        getItem(value, slug);
    
      });
    });

    item.subscribe((value) => {
      console.log(value);
      foundItem = value;
    });
  });
</script>

{#if foundItem.name}
  <h1>{foundItem.name}</h1>
{/if}
{#if foundItem.media}
  <!-- svelte-ignore a11y-missing-attribute -->
  <img src={foundItem.media} />
{/if}
<ul>
  {#each filteredAuctions as auction}
    <li>
      {auction.buyoutParsed}
    </li>
  {/each}
</ul>
