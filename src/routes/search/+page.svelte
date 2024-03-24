<script>
  import { page } from "$app/stores";
  import { onMount } from "svelte";
  import {
    getSearchResults,
    getToken,
    searchResults,
    authToken,
  } from "../getToken.svelte";

  const query = $page.url.searchParams.get("q");
  let filteredResults = [];

  onMount(async () => {
    getToken().then(() => {
      authToken.subscribe((value) => {
        getSearchResults(value, query);
      });
    });

    searchResults.subscribe((value) => {
      filteredResults = value;
    });
  });
</script>

{#each filteredResults as item}
  <a href="auctions/{item.data.id}">
    <h1>{item.data.name.en_US}</h1>

    {#if item.data.media}
      <!-- svelte-ignore a11y-missing-attribute -->
      <img src={item.data.media} />
    {/if}
  </a>
{/each}
