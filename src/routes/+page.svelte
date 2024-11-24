<script>
    import Map from '$lib/components/Map.svelte';
    import SearchBar from '$lib/components/SearchBar.svelte';
    import ResourceCard from '$lib/components/ResourceCard.svelte';
    import Header from '$lib/components/Header.svelte';
    import Footer from '$lib/components/Footer.svelte';
  
    let resources = [
      {
        name: "Local Food Bank",
        address: "123 Main St, Anytown",
        status: "Open",
      },
      {
        name: "Community Kitchen",
        address: "456 Elm St, Anytown",
        status: "Limited Stock",
      }
    ];
  
    const searchResources = (query) => {
      console.log('Search for:', query);
      // Add search/filter functionality here
    };
</script>

<Header title="FoodMap" />
  
<SearchBar onSearch={searchResources} />
<div style="height: 100%; width: 100%; display: block;">
    <Map />
</div>

<div>
    {#each resources as resource}
        <ResourceCard {resource} />
    {/each}
</div>

<Footer />