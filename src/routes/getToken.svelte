<script context="module">
  import axios from "axios";
  import { writable, readable } from "svelte/store";

  export const authToken = writable("");
  export const auctions = writable([]);
  export const item = writable({});
  export const searchResults = writable([]);

  export async function getToken() {
    const response = await axios.get("https://wow-ah-server.onrender.com/");
    authToken.set(response.data.access_token);
    return {
      props: {
        authToken: response.data.access_token,
      },
    };
  }

  export async function getSearchResults(value, query) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/search/item?namespace=static-classic-us&locale=en_US&orderby=id:asc&access_token=${value}&name.en_US=${query}`
    )
      .then((response) => response.json())
      .then((data) => {
        searchResults.set(data.results);
        data.results.forEach((result) => {
          getSearchMedia(value, result.data.id);
        });
      });
  }

  export async function getSearchMedia(value, id) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/media/item/${id}?namespace=static-classic-us&locale=en_US&access_token=${value}`
    )
      .then((response) => response.json())
      .then((data) => {
        searchResults.update((n) => {
          n.forEach((result) => {
            if (result.data.id === id) {
              result.data.media = data.assets[0].value;
            }
          });
          return n;
        });
      });
  }

  export async function getItem(value, slug) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/item/${slug.slug}?namespace=static-classic-us&locale=en_US&access_token=${value}`
    )
      .then((response) => response.json())
      .then((data) => {
        item.set(data);
        getMedia(value, data.media.id);
      });
  }

  export async function getMedia(value, id) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/media/item/${id}?namespace=static-classic-us&locale=en_US&access_token=${value}`
    )
      .then((response) => response.json())
      .then((data) => {
        item.update((n) => {
          n.media = data.assets[0].value;
          return n;
        });
      });
  }
</script>
