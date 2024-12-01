<script>
    import Header from '$lib/components/Header.svelte';
    import Footer from '$lib/components/Footer.svelte';

    const goBack = () => {
      history.back();
    };
  </script>
  
  <style>
    .about-container {
      max-width: 800px;
      margin: 2rem auto;
      padding: 2rem;
      border: 1px solid #ddd;
      border-radius: 12px;
      background-color: white;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      font-family: 'Poppins', sans-serif;
      text-align: center;
      position: relative;
    }
  
    h1 {
      font-size: 2rem;
      margin-bottom: 1rem;
      color: #333;
    }
  
    p {
      font-size: 1rem;
      line-height: 1.6;
      margin-bottom: 1.5rem;
      color: #555;
    }
  
    .highlight {
      color: #6AB04C;
      font-weight: bold;
    }
  
    .logo {
      width: 250px;
      margin: 1rem auto;
      display: block;
    }

    .back-button {
      position: absolute;
      top: 10px;
      left: 10px;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background-color: #f0932b; /* Orange background */
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      cursor: pointer;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
      border: none;
    }
  
    .back-button:hover {
      background-color: #d87c25; /* Darker orange on hover */
    }
  
    .back-icon {
      font-size: 1.2rem;
      margin-bottom: 4px;
    }
  </style>
  
  <Header title="FoodMap" />
  
  <div class="about-container">
    <!-- Back Button -->
    <button class="back-button" on:click={goBack}>
        <span class="back-icon">←</span>
    </button>

    <img src="/logo-text.png" alt="FoodMap Logo" class="logo" />
    <h1>About FoodMap</h1>
    <p>
      <span class="highlight">FoodMap</span> is an initiative designed to help connect individuals and families
      with essential food resources in their community. Whether you're looking for food banks, community kitchens,
      or other services, <span class="highlight">FoodMap</span> aims to make it easy to find and access support.
    </p>
    <p>
      Our platform is committed to combating food insecurity and promoting well-being by providing:
    </p>
    <ul>
      <li>Accurate and up-to-date information on food resources near you.</li>
      <li>Easy-to-use navigation tools to locate resources quickly.</li>
      <li>Donation options to help others in need.</li>
    </ul>
    <p>
      Together, we can make a difference and ensure that no one goes hungry. Thank you for supporting our mission!
    </p>
  </div>
  
  <Footer />
  