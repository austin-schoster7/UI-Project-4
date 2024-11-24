<script>
    export let resource = {
      name: 'Resource Name',
      address: '123 Main Street',
      status: 'Open', // Options: Open, Limited, Closed
    };
  </script>
  
  <style>
    .card {
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 1.5rem;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      font-family: 'Poppins', sans-serif;
    }
  
    .card:hover {
      transform: translateY(-4px);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
    }
  
    .status {
      font-weight: bold;
      margin-top: 0.5rem;
    }
  
    .status.green {
      color: #6AB04C;
    }
  
    .status.orange {
      color: #F0932B;
    }
  
    .status.red {
      color: #EB4D4B;
    }
  </style>
  
  <div class="card">
    <h3>{resource.name}</h3>
    <p>{resource.address}</p>
    <p class="status {resource.status.toLowerCase()}">
      Status: {resource.status}
    </p>
  </div>
  