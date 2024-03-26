<script>
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  import Navigation from '../components/Navigation.svelte';


  let searchQuery = '';

  const handleEnterPress = (event) => {
    if (event.key === 'Enter') {
      goto('/search/?q=' + searchQuery);
    }
  };

</script>

<Navigation />

<section class="main-section">
  <div class="header">
    <div class="title-container">
      <h1 class="main-title">Explore  Tichondrius items</h1>
  
      <input class="search-input" type="search" name="search" placeholder="Search for something in World of Warcraft... anything" bind:value={searchQuery} on:keydown={handleEnterPress}>

    </div>
  </div>
  
  <section class="stats-section">
    <ul class="stats-list">
      <li class="item-count stats-item"><span class="item-count-text">Check out prices here</li>
     
    </ul>
  </section>
</section>
<style>
  * {
    font-family: 'Poppins', sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .main-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem 8%;
  }

  .header {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .title-container {
    width: 100%;
    text-align: center;
  }

  .main-title {
    color: #fff;
    font-size: 2rem;
    font-weight: 700;
    line-height: 1.2;
    margin-bottom: 2rem;
  }

  .item-count {
    font-weight: 700;
    color: #B9B9B9;
  }

  .search-input {
    width: 31.25rem; 
    padding: 1rem 1.5625rem; 
    box-sizing: border-box;
    background-color: #ddc3c3;
    border: none;
    border-radius: 999rem;
    color: white;
    font-size: 14px;
  }

  .search-input::placeholder {
    opacity: 1;
    color: rgb(153, 153, 153);
  }

  .stats-section {
    width: 80%;
    margin: 3rem auto;
  }

  .stats-list {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
  }

  .stats-item {
    color: #B9B9B9;
    font-size: 22px;
    font-weight: 600;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .item-count-text {
    color: #fff;
    font-size: 2rem;
    font-weight: bold;
  }

  @media screen and (min-width: 1600px) {
    .main-title {
      font-size: 6.25rem; 
    }

    .search-input {
      padding: 1.25rem 1.5625rem; 
      font-size: 15px;
    }
  }
</style>
