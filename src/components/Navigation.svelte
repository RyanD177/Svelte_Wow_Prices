<script>
  import { goto } from "$app/navigation";

  let searchQuery = "";


  
</script>

<header class="header">
  <nav class="header__nav">
    <ul class="header__list">
      <li class="header__item"><a href="/" class="header__link">Home</a></li>
    </ul>
  </nav>
</header>

<style>
  .header {
    width: 100%;

    height:4%;
    padding: 2rem 0;
    position: relative;
  }

  .header__nav {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .header__list {
    display: flex;
    margin-left: auto;
    right: 10%;
    position: absolute;
    gap: 2rem;
  }

  .header__link {
    color: white;
    text-decoration: none;
    font-family: "Poppins", sans-serif;
  }
</style>
