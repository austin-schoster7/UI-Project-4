<script>
    import { page } from '$app/stores';
    import Header from '$lib/components/Header.svelte';
    import Footer from '$lib/components/Footer.svelte';
    import { goto } from '$app/navigation';
    import DonationPopup from '$lib/components/DonationPopup.svelte';
  
    // Retrieve query parameters
    $: resourceName = $page.url.searchParams.get('resourceName') || 'No resource name found';
    $: address = $page.url.searchParams.get('address') || 'No address found';
    $: lat = $page.url.searchParams.get('lat') || null;
    $: lng = $page.url.searchParams.get('lng') || null;

    // State for showing the popup
    let showPopup = false;
  
    // Handle "Get Directions" button
    const onGetDirections = () => {
      if (lat && lng) {
        const url = `https://www.google.com/maps/search/?api=1&query=${lat},${lng}`;
        window.open(url, '_blank');
      } else {
        alert('Location data is unavailable.');
      }
    };
  
    // Navigate back to the previous page
    const goBack = () => {
      history.back();
    };

    // Handle form submission
    const handleDonation = () => {
        showPopup = true; // Show the popup
    };
</script>
  
<style>
    .donation-container {
        position: relative; /* Make the container the positioning context */
        max-width: 600px;
        margin: 2rem auto;
        padding: 2rem;
        border: 1px solid #ddd;
        border-radius: 12px;
        background-color: white;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        font-family: 'Poppins', sans-serif;
        text-align: center;
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

    h2 {
        margin-bottom: 1rem;
        color: #333;
    }

    form {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    label {
        font-weight: bold;
        text-align: left;
    }

    .required {
        color: red;
        font-weight: normal;
        padding-left: 2px;
    }

    input,
    button {
        padding: 0.75rem;
        border-radius: 8px;
        border: 1px solid #ddd;
        font-family: 'Poppins', sans-serif;
    }

    input {
        border-color: #6AB04C;
    }

    input:focus {
        outline: none;
        border-color: #f0932b; /* Orange accent */
        /* border-width: 3px; */
    }

    button {
        background-color: #6AB04C;
        color: white;
        cursor: pointer;
        border: none;
    }

    button:hover {
        background-color: #5a9b3e;
    }

    .pin-logo {
        width: 100px;
        height: 140px;
        flex-shrink: 0;
    }

    .directions-button {
        background-color: #f0932b; /* Orange for Get Directions */
        color: white;
        cursor: pointer;
        border: none;
        padding: 0.75rem;
        border-radius: 8px;
        width: 100%;
    }

    .directions-button:hover {
        background-color: #d87c25;
    }
</style>
  
<Header title="FoodMap" />
  
<div class="donation-container">
<!-- Back Button -->
    <button class="back-button" on:click={goBack}>
        <span class="back-icon">←</span>
    </button>

    <!-- Pin Logo -->
    <img class="pin-logo" src="/pin-transparent.png" alt="Location Pin" />
    <h2>Donate to {resourceName}</h2>
    <p>Your contribution helps {resourceName} provide essential services to the community. If you would like to make a food donation, please bring it to {address}</p>

    <form on:submit|preventDefault={handleDonation}>
        <label for="amount">
        Donation Amount ($):<span class="required">*</span>
        </label>
        <input id="amount" type="number" min="1" placeholder="Enter amount" required />

        <label for="name">
        Your Name:<span class="required">*</span>
        </label>
        <input id="name" type="text" placeholder="Enter your name" required />

        <button type="submit">Donate</button>
        <button class="directions-button" on:click={onGetDirections}>Get Directions</button>
    </form>
</div>

  <!-- Show the popup if donation is successful -->
{#if showPopup}
    <DonationPopup message="Thank you for donating to {resourceName}!" />
{/if}
  
<Footer />
  