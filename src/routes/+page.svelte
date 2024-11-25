<script>
    import Map from '$lib/components/Map.svelte';
    import SearchBar from '$lib/components/SearchBar.svelte';
    import ResourceCard from '$lib/components/ResourceCard.svelte';
    import Header from '$lib/components/Header.svelte';
    import Footer from '$lib/components/Footer.svelte';
  
    let showFilterModal = false;
  
    let resources = [
      { name: "Local Food Bank", address: "123 Main St, Anytown", status: "Open" },
      { name: "Community Kitchen", address: "456 Elm St, Anytown", status: "Limited Stock" },
      { name: "Homeless Shelter", address: "789 Oak Ave, Anytown", status: "Closed" }
    ];
  
    let filteredResources = [...resources];
    let filter = {
      status: '',
      name: ''
    };
  
    // Search resources by name or address
    const searchResources = (query) => {
      filteredResources = resources.filter((r) =>
        r.name.toLowerCase().includes(query.toLowerCase()) ||
        r.address.toLowerCase().includes(query.toLowerCase())
      );
    };
  
    // Apply filters based on status and name
    const filterResources = () => {
      const statusFilter = filter.status.toLowerCase();
      const nameFilter = filter.name.toLowerCase();
  
      filteredResources = resources.filter((r) => {
        const matchesStatus = statusFilter ? r.status.toLowerCase().includes(statusFilter) : true;
        const matchesName = nameFilter ? r.name.toLowerCase().includes(nameFilter) : true;
        return matchesStatus && matchesName;
      });
    };
  
    // Remove a specific filter (status or name) and reapply filters
    const removeFilter = (type) => {
      filter[type] = ''; // Clear the specific filter
      filterResources(); // Reapply filters
    };
  </script>
  
  <style>
    .filter-modal {
      position: absolute;
      top: 100px;
      left: 50%;
      transform: translateX(-50%);
      background: white;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      padding: 1.5rem;
      z-index: 1000;
      width: 300px;
      font-family: 'Poppins', sans-serif;
    }
  
    .filter-modal label {
      display: block;
      margin: 0.5rem 0;
    }
  
    .filter-modal button {
      margin-top: 1rem;
      background: #F0932B;
      border: none;
      color: white;
      padding: 0.75rem 1rem;
      border-radius: 8px;
      cursor: pointer;
    }
  
    .filter-modal button:hover {
      background: #D87C25;
    }
  
    .filter-modal select {
      width: 100%;
      padding: 0.5rem;
      border-radius: 8px;
      border: 1px solid #ddd;
      font-family: 'Poppins', sans-serif;
    }
  
    .filter-modal input {
      width: 100%;
      padding: 0.5rem;
      border-radius: 8px;
      border: 1px solid #ddd;
      font-family: 'Poppins', sans-serif;
    }
  
    .active-filters {
      display: flex;
      gap: 1rem;
      margin: 1rem 2rem;
      font-family: 'Poppins', sans-serif;
    }
  
    .filter-tag {
      background: #f1f1f1;
      border: 1px solid #ddd;
      border-radius: 16px;
      padding: 0.5rem 1rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-size: 0.9rem;
    }
  
    .filter-tag button {
      background: none;
      border: none;
      color: #888;
      cursor: pointer;
      font-size: 1rem;
    }
  
    .filter-tag button:hover {
      color: #d00; /* Red for hover effect */
    }
  
    h2 {
      font-family: 'Poppins', sans-serif;
      font-size: 1.5rem;
      margin: 2rem 0 1rem;
    }
  </style>
  
  <Header title="FoodMap" />
  
  <SearchBar
    onSearch={searchResources}
    onFilter={() => showFilterModal = !showFilterModal}
  />
  
  <!-- Active Filters Section -->
  {#if filter.status || filter.name}
    <div class="active-filters">
      {#if filter.status}
        <span class="filter-tag">
          Status: {filter.status}
          <button on:click={() => removeFilter('status')}>✖</button>
        </span>
      {/if}
      {#if filter.name}
        <span class="filter-tag">
          Name: {filter.name}
          <button on:click={() => removeFilter('name')}>✖</button>
        </span>
      {/if}
    </div>
  {/if}
  
  <!-- Filter Modal -->
  {#if showFilterModal}
    <div class="filter-modal">
      <label>
        Filter by Status:
        <select bind:value={filter.status}>
          <option value="">All</option>
          <option value="Open">Open</option>
          <option value="Limited Stock">Limited Stock</option>
          <option value="Closed">Closed</option>
        </select>
      </label>
  
      <label>
        Filter by Name:
        <input type="text" bind:value={filter.name} placeholder="Enter name..." />
      </label>
  
      <button on:click={() => { filterResources(); showFilterModal = false; }}>Apply Filter</button>
    </div>
  {/if}
  
  <div style="height: 100%; width: 100%; display: block;">
    <Map />
  </div>
  
  <h2>Nearby Food Resources</h2>
  
  <div>
    {#each filteredResources as resource}
      <ResourceCard {resource} />
    {/each}
  </div>
  
  <Footer />
  