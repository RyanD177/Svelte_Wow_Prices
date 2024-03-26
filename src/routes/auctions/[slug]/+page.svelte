<script>
  import { page } from "$app/stores";
  import { onMount } from "svelte";
  import {
    auctions,
    getAuctions,
    fetchItem,
    getProps,
    token,
    item as selectedItem,
  } from "../../getToken.svelte";
  import Navigation from "../../../components/Navigation.svelte";

  const slugParam = $page.params;
  let filteredAuctions = [];
  let selectedItemInfo = {};
  let loadingStatus = true;

  onMount(async () => {
    getProps().then(() => {
      token.subscribe((value) => {
        fetchItem(value, slugParam);
        getAuctions(value);
      });
    });

    selectedItem.subscribe((value) => {
      console.log(value);
      selectedItemInfo = value;
    });

    auctions.subscribe((value) => {
      console.log(value);
      loadingStatus = false;
      filteredAuctions = value;
      filteredAuctions.sort((a, b) => a.buyout - b.buyout);
    });
  });
</script>

<Navigation />

<section class="auction__item">
  {#if selectedItemInfo.media}
    <img src={selectedItemInfo.media} alt="Icon for {selectedItemInfo.name}" />
  {/if}
  <div class="auction__info">
    {#if selectedItemInfo.name}
      <h1
        class="auction__name"
        style="color: red; list-style:none; text-decoration:none;"
      >
        {selectedItemInfo.name}
      </h1>
      <div class="auction__data">
        <h2 class="auction__quality auction__quality--{selectedItemInfo.quality.name}">
          {selectedItemInfo.quality.name}
        </h2>
        <span>/</span>
        <h2 class="auction__class">{selectedItemInfo.item_class.name}</h2>
      </div>
    {:else}
      <h1>Loading your item</h1>
    {/if}
  </div>
</section>
