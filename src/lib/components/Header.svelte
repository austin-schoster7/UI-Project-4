<script>
    export let title = 'FoodMap'; // Optional, default app title
  </script>
  
  <style>
    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 1rem 2rem;
      background-color: white;
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
      height: 60px; /* Adjusted size for a larger logo */
      margin-right: 1rem;
    }
  
    .logo h1 {
      font-size: 1.75rem; /* Adjusted size for the title */
      margin: 0;
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
      background-color: #f5f5f5; /* Subtle hover effect */
      color: #6AB04C; /* Green accent */
    }
  </style>
  
  <header>
    <div class="logo">
      <img src="/logo-text.png" alt="FoodMap Logo" />
      <h1>{title}</h1>
    </div>
    <nav class="nav">
      <a href="#map">Map</a>
      <a href="#resources">Resources</a>
      <a href="#about">About</a>
    </nav>
  </header>
  