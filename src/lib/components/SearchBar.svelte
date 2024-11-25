<script>
    export let placeholder = "Search for food resources...";
    export let onSearch = (query) => console.log("Searching for:", query);
    export let onFilter = () => console.log("Filter clicked");
    let query = '';
  </script>
  
  <style>
    .search-bar {
      display: flex;
      margin: 2rem auto;
      justify-content: center;
      align-items: center;
      gap: 1rem;
    }
  
    input {
      width: 50%;
      padding: 0.75rem 1rem;
      border: 2px solid #6AB04C; /* Green border */
      border-radius: 25px;
      font-size: 1rem;
      outline: none;
    }
  
    input:focus {
      border-color: #f0932b; /* Orange accent */
    }
  
    button {
      padding: 0.75rem 1rem;
      background: #6AB04C; /* Green background */
      border: none;
      color: white;
      font-size: 1rem;
      border-radius: 25px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }
  
    button:hover {
      background: #4E9B3B; /* Darker green */
    }
  
    .filter-button {
      background: #F0932B; /* Orange for filter button */
    }
  
    .filter-button:hover {
      background: #D87C25; /* Darker orange */
    }
  
    .filter-button {
      font-size: 1.2rem;
    }

    .filter-button img {
      max-height: 20px;
    }

    .search-button {
      font-size: 1.2rem;
      max-height: 55px;
    }
  </style>
  
  <div class="search-bar">
    <input
      type="text"
      placeholder={placeholder}
      bind:value={query}
      on:keyup={(e) => e.key === 'Enter' && onSearch(query)}
    />
    <button class="search-button" on:click={() => onSearch(query)}>Search</button>
    <button class="filter-button" on:click={onFilter}>
      <img src="/filter.png" alt=""> Filter
    </button>
  </div>
  