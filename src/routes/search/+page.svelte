<script>
  import { page } from "$app/stores";
  import { onMount } from "svelte";
  import {
    getSearchResults,
    getServerSideProps,
    searchResults,
    token,
  } from "../getToken.svelte";
  import Navigation from "../../components/Navigation.svelte";


  const query = $page.url.searchParams.get("q");
  let filteredResults = [];


  onMount(async () => {
    getServerSideProps().then(() => {
      token.subscribe((value) => {
        getSearchResults(value, query);
      });
    });
    searchResults.subscribe((value) => {
      filteredResults = value;
      console.log(filteredResults);
      filteredResults.sort((a, b) => {
        if (a.data.name.en_US < b.data.name.en_US) {
          return -1;
        }
        if (a.data.name.en_US > b.data.name.en_US) {
          return 1;
        }
        return 0;
      });
    });
  });
</script>

<Navigation />
{#each filteredResults as item}
  <a href="auctions/{item.data.id}">
    <h1 style="color: red; text-decoration:none; ">{item.data.name.en_US}</h1>

    {#if item.data.media}
      <!-- svelte-ignore a11y-missing-attribute -->
      <img src={item.data.media} />
    {/if}
  </a>
{/each}
