<script>
    export let placeholder = "Search for food resources...";
    export let onSearch = (query) => console.log("Searching for:", query);
    let query = '';
  </script>
  
  <style>
    .search-bar {
      display: flex;
      margin: 2rem auto;
      justify-content: center;
    }
  
    input {
      width: 60%;
      padding: 0.75rem 1rem;
      border: 2px solid #6AB04C; /* Green border */
      border-radius: 25px;
      font-size: 1rem;
      outline: none;
    }
  
    input:focus {
      border-color: #F0932B; /* Orange accent on focus */
    }
  
    button {
      margin-left: 1rem;
      padding: 0.75rem 1rem;
      background: #6AB04C; /* Green background */
      border: none;
      color: white;
      font-size: 1rem;
      border-radius: 25px;
      cursor: pointer;
    }
  
    button:hover {
      background: #4E9B3B; /* Darker green */
    }
  </style>
  
  <div class="search-bar">
    <input
      type="text"
      placeholder={placeholder}
      bind:value={query}
      on:keyup={(e) => e.key === 'Enter' && onSearch(query)}
    />
    <button on:click={() => onSearch(query)}>Search</button>
  </div>
  