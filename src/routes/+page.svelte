<script>
  import Map from '$lib/components/Map.svelte';
  import SearchBar from '$lib/components/SearchBar.svelte';
  import ResourceCard from '$lib/components/ResourceCard.svelte';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';

  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let showFilterModal = false;

  let resources = [
    {
      name: "Local Food Bank",
      address: "123 Main St, Cincinnati, OH",
      status: "Open",
      location: { lat: 39.1031, lng: -84.5120 },
      services: ["Canned Goods", "Clothing"],
    },
    {
      name: "Community Kitchen",
      address: "456 Elm St, Cincinnati, OH",
      status: "Limited Stock",
      location: { lat: 39.1095, lng: -84.5207 },
      services: ["Warm Meals", "Canned Goods"],
    },
    {
      name: "Homeless Shelter",
      address: "789 Oak Ave, Cincinnati, OH",
      status: "Closed",
      location: { lat: 39.1115, lng: -84.5165 },
      services: ["Shelter", "Medical Aid", "Warm Meals"],
    },
  ];

  let filteredResources = [...resources];
  let filter = { status: '', name: '', service: '' };
  let mapRef;
  let mapElement;

  const searchResources = (query) => {
    filteredResources = resources.filter((r) =>
      r.name.toLowerCase().includes(query.toLowerCase()) ||
      r.address.toLowerCase().includes(query.toLowerCase())
    );

    if (mapRef && mapRef.setResourceMarkers) {
      mapRef.setResourceMarkers(filteredResources.map(r => ({
        name: r.name,
        position: r.location,
        status: r.status,
      })));
    }
  };

  const filterResources = () => {
    const statusFilter = filter.status.toLowerCase();
    const nameFilter = filter.name.toLowerCase();
    const serviceFilter = filter.service;

    filteredResources = resources.filter((r) => {
      const matchesStatus = statusFilter ? r.status.toLowerCase().includes(statusFilter) : true;
      const matchesName = nameFilter ? r.name.toLowerCase().includes(nameFilter) : true;
      const matchesService = serviceFilter ? r.services.includes(serviceFilter) : true;
      return matchesStatus && matchesName && matchesService;
    });

    if (mapRef && mapRef.setResourceMarkers) {
      mapRef.setResourceMarkers(filteredResources.map(r => ({
        name: r.name,
        position: r.location,
        status: r.status,
      })));
    }
  };

  const removeFilter = (type) => {
    filter[type] = '';
    filterResources();
  };

  // Center the map on the selected resource
  const onCenterMap = (location) => {
    if (mapRef && mapRef.centerOnLocation) {
      mapRef.centerOnLocation(location);
    }

    // Scroll to the map component
    if (mapElement) {
      mapElement.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
  };

  const onGetDirections = (resource) => {
    const { lat, lng } = resource.location;
    const url = `https://www.google.com/maps/dir/?api=1&destination=${lat},${lng}`;
    window.open(url, '_blank');
  };

  const onDonate = (resource) => {
    goto(`/donate/?resourceName=${encodeURIComponent(resource.name)}&address=${resource.address}&lat=${resource.location.lat}&lng=${resource.location.lng}`);
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
{#if filter.status || filter.name || filter.service}
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
    {#if filter.service}
      <span class="filter-tag">
        Service: {filter.service}
        <button on:click={() => removeFilter('service')}>✖</button>
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
      Filter by Service:
      <select bind:value={filter.service}>
        <option value="">All</option>
        <option value="Canned Goods">Canned Goods</option>
        <option value="Warm Meals">Warm Meals</option>
        <option value="Shelter">Shelter</option>
        <option value="Clothing">Clothing</option>
        <option value="Medical Aid">Medical Aid</option>
      </select>
    </label>

    <label>
      Filter by Name:
      <input type="text" bind:value={filter.name} placeholder="Enter name..." />
    </label>

    <button on:click={() => { filterResources(); showFilterModal = false; }}>Apply Filter</button>
  </div>
{/if}

<div style="height: 100%; width: 100%; display: block;" bind:this={mapElement}>
  <Map bind:this={mapRef} />
</div>

<h2>Nearby Food Resources</h2>

<div id="resource-cards">
  {#each filteredResources as resource}
    <ResourceCard
      {resource}
      onCenterMap={() => onCenterMap(resource.location)}
      onGetDirections={onGetDirections}
      onDonate={onDonate}
    />
  {/each}
</div>

<Footer />
  