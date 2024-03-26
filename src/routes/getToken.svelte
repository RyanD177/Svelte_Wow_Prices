<script context="module">
  import axios from "axios";
  import { writable } from "svelte/store";
  // writable function:  creates a store  that can be set from outer components
  export const token = writable("");
  export const items = writable([]);
  export const auctions = writable([]);
  export const item = writable({});
  export const searchResults = writable([]);

  export async function getProps() {
    const response = await axios.get("https://wow-ah-server.onrender.com/");
    token.set(response.data.access_token);
    return {
      props: {
        token: response.data.access_token,
      },
    };
  }

  export async function getItems() {
    const response = await axios.get(
      "https://wow-ah-server.onrender.com/items"
    );
    items.set(response.data);
    return {
      props: {
        items: response.data,
      },
    };
  }

  export async function getSearchResults(value, query) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/search/item?namespace=static-classic-us&locale=en_US&orderby=id:asc&name.en_US=${query}`,
      {
        headers: {
          Authorization: `Bearer ${value}`,
        },
      }
    )
      .then((response) => response.json())
      .then((data) => {
        data.results = data.results.filter((item) => {
          return (
            // get rid of items with test in the name
            !item.data.name.en_US.includes("TEST") &&
            !item.data.name.en_US.includes("Test")
          );
        });

        searchResults.set(data.results);
        data.results.forEach((result) => {
          getSearchMedia(value, result.data.id);
        });
      });
  }

  export async function getSearchMedia(value, id) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/media/item/${id}?namespace=static-classic-us&locale=en_US&`,
      {
        headers: {
          Authorization: `Bearer ${value}`,
        },
      }
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

  export async function fetchItem(value, slug) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/item/${slug.slug}?namespace=static-classic-us&locale=en_US`,
      {
        headers: {
          Authorization: `Bearer ${value}`,
        },
      }
    )
      .then((response) => response.json())
      .then((data) => {
        item.set(data);
        getMedia(value, data.media.id);
      });
  }

  export async function getMedia(value, id) {
    await fetch(
      `https://us.api.blizzard.com/data/wow/media/item/${id}?namespace=static-classic-us&locale=en_US`,
      {
        headers: {
          Authorization: `Bearer ${value}`,
        },
      }
    )
      .then((response) => response.json())
      .then((data) => {
        item.update((n) => {
          n.media = data.assets[0].value;
          return n;
        });
      });
  }

  export async function getAuctions(value) {
    const url = `https://us.api.blizzard.com/data/wow/connected-realm/4384/auctions/2?namespace=dynamic-classic-us&locale=en_US&access_token=${value}`;

    await fetch(url, {
      headers: {
        Authorization: `Bearer ${value}`,
      },
    })
      .then((response) => response.json())
      .then((data) => {
        auctions.set(data.auctions);
      })
      .catch((error) => {
        console.error("Error fetching auctions:", error);
      });
  }
</script>
