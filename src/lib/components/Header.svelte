<script>
  import { page } from '$app/stores';
  export let title = 'FoodMap'; // Default title

  // Scroll to the resource cards section
  const scrollToResources = () => {
    const resourceSection = document.getElementById('resource-cards');
    if (resourceSection) {
      resourceSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
  };

  // Reactive variable to check if the current route is the home page
  $: isHomePage = $page.url.pathname === '/';
</script>

<style>
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 2rem;
    background-color: #f9f9f9;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    position: sticky;
    top: 0;
    z-index: 1000;
    font-family: 'Poppins', sans-serif;
  }

  .logo {
    display: flex;
    align-items: center;
  }

  .logo img {
    height: 60px;
    margin-right: 1rem;
  }

  .logo a {
    text-decoration: none;
    color: #333;
    font-size: 1.5rem;
    font-weight: bold;
  }

  .logo a:hover {
    color: #6AB04C; /* Highlight color on hover */
  }

  .nav {
    display: flex;
    gap: 1rem;
  }

  .nav a {
    text-decoration: none;
    color: #333;
    font-weight: 500;
    padding: 0.5rem 1rem;
    border-radius: 5px;
  }

  .nav a:hover {
    background-color: #e4e4e4; /* Subtle hover effect */
    color: #6AB04C; /* Green accent */
  }
</style>

<header>
  <div class="logo">
    <img src="/logo-text.png" alt="FoodMap Logo" />
    <a href="/">{title}</a>
  </div>
  <nav class="nav">
    <a href="/">Home</a>
    {#if isHomePage}
      <a href="javascript:void(0);" on:click={scrollToResources}>Resources</a>
    {/if}
    <a href="/about">About</a>
  </nav>
</header>
