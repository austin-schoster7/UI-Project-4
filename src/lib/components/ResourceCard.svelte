<script>
    export let resource = {
      name: 'Resource Name',
      address: '123 Main Street',
      status: 'Open', // Options: Open, Limited Stock, Closed
    };
  </script>
  
  <style>
    .card {
        background: #f9f9f9 url('/pin-transparent.png') no-repeat top right;
        background-size: 50px;
        border: 1px solid #ddd; /* Subtle border for better contrast */
        border-radius: 12px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        padding: 1.5rem;
        transition: transform 0.2s ease, box-shadow 0.2s ease;
        font-family: 'Poppins', sans-serif;
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
    }
  
    .card:hover {
        transform: translateY(-4px);
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
    }
  
    .header {
        display: flex;
        align-items: center;
        gap: 0.5rem; /* Space between pin logo and text */
    }
  
    .header img {
        height: 32px; /* Pin logo size */
        width: 28px;
        transition: transform 0.2s ease, opacity 0.2s ease;
    }
  
    .status {
        font-weight: bold;
        margin-top: 0.5rem;
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
  </style>
  
  <div class="card">
    <div class="header">
      <img src="/pin-transparent.png" alt="Location Pin" />
      <h3>{resource.name}</h3>
    </div>
    <p>{resource.address}</p>
    <p class="status {resource.status.toLowerCase().replace(' ', '-')}">
      Status: {resource.status}
    </p>
  </div>
  