<script>
  export let resource = {
    name: 'Resource Name',
    address: '123 Main Street',
    services: ['Service 1', 'Service 2', 'Service 3'],
    status: 'Open',
    location: { lat: 0, lng: 0 }, // Latitude and Longitude of the resource
  };

  export let onCenterMap = () => {};
  export let onGetDirections = () => {};
  export let onDonate = () => {};
</script>

<style>
  .card {
    display: flex;
    align-items: center;
    gap: 1rem;
    background: #f9f9f9;
    border: 1px solid #ddd;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 1.5rem;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    font-family: 'Poppins', sans-serif;
    cursor: pointer;
  }

  .card:hover {
    transform: translateY(-4px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  }

  .pin-logo {
    width: 50px;
    height: 65px;
    flex-shrink: 0; /* Prevents the logo from shrinking */
  }

  .content {
    flex-grow: 1; /* Ensures content takes up the remaining space */
  }

  .buttons {
    display: flex;
    gap: 0.5rem;
    margin-top: 1rem;
  }

  .buttons button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 8px;
    font-size: 0.9rem;
    cursor: pointer;
    font-family: 'Poppins', sans-serif;
  }

  .donate-button {
    background: #6ab04c; /* Green for directions */
    color: white;
  }

  .donate-button:hover {
    background: #4e9b3b;
  }

  .directions-button {
    background: #f0932b; /* Orange for donations */
    color: white;
  }

  .directions-button:hover {
    background: #d87c25;
  }

  .status {
    font-weight: bold;
    margin-top: 1rem;
  }

  .status.open {
    color: #6AB04C; /* Green for "Open" */
  }
  
  .status.limited-stock {
    color: #F0932B; /* Orange for "Limited Stock" */
  }

  .status.closed {
    color: #EB4D4B; /* Red for "Closed" */
  }

  .services {
    margin-top: 1rem;
    text-align: left;
  }

  .services ul {
    list-style-type: disc;
    margin: 0;
    padding-left: 1.5rem;
  }

  .services li {
    font-size: 0.9rem;
    color: #555;
  }
</style>

<div class="card" on:click={() => onCenterMap(resource.location)}>
  <!-- Pin Logo -->
  <img class="pin-logo" src="/pin-transparent.png" alt="Location Pin" />
  
  <!-- Resource Content -->
  <div class="content">
    <h3>{resource.name}</h3>
    <p>{resource.address}</p>
    <div class="services">
      <p>Available Services:</p>
      <ul>
        {#each resource.services as service}
          <li>{service}</li>
        {/each}
      </ul>
    </div>
    <p class="status {resource.status.toLowerCase().replace(' ', '-')}">
      Status: {resource.status}
    </p>
    <div class="buttons">
      <button class="donate-button" on:click={(e) => { e.stopPropagation(); onDonate(resource); }}>
        Donate
      </button>
      <button class="directions-button" on:click={(e) => { e.stopPropagation(); onGetDirections(resource); }}>
        Get Directions
      </button>
    </div>
  </div>
</div>
