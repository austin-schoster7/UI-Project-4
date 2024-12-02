<script>
    import { goto } from '$app/navigation';

    export let message = "Thank you for your donation!";
    export let redirectTo = "/"; // Default redirect to the main page

    const closePopup = () => {
        goto(redirectTo);
    };
</script>

<style>
    .popup-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.6);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 1000;
    }

    .popup {
        background: white;
        padding: 2rem;
        border-radius: 12px;
        text-align: center;
        max-width: 400px;
        width: 90%;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        font-family: 'Poppins', sans-serif;
    }

    .popup h2 {
        margin-bottom: 1rem;
        color: #333;
    }

    .popup button {
        padding: 0.75rem 1.5rem;
        border: none;
        border-radius: 8px;
        background: #6AB04C; /* Green button */
        color: white;
        cursor: pointer;
        margin-top: 1rem;
    }

    .popup button:hover {
        background: #5a9b3e;
    }
</style>

<div class="popup-overlay">
    <div class="popup">
        <h2>{message}</h2>
        <p>Your contribution helps us make a difference.</p>
        <button on:click={closePopup}>Back to Main Menu</button>
    </div>
</div>
  