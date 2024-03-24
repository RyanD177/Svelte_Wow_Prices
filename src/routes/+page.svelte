<script>
  import { goto } from '$app/navigation';
  let searchQuery = '';

  const handleEnterPress = (event) => {
    if (event.key === 'Enter') {
      goto('/search/?q=' + searchQuery);
    }
  };
</script>

<style>
  .search-page {
    background-image: url('./photos/632fcf4b-094c-4d3f-97c4-a9ee8d634981.webp');
    background-size: cover;
    background-position: center;
    width: 100vw;
    height: 100vh;
  }
</style>

<div class="search-page">
  <input 
    type="search" 
    name="search" 
    placeholder="Search" 
    bind:value={searchQuery}
    on:keydown={handleEnterPress}
  >
</div>
