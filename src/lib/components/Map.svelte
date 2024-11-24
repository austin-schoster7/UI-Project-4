<script>
    import { onMount } from 'svelte';
  
    // Custom map styles
    const styles = [
      {
        featureType: 'water',
        stylers: [{ color: '#0e1626' }],
      },
      {
        featureType: 'landscape',
        stylers: [{ color: '#1e303d' }],
      },
    ];
  
    let map;
  
    // Food resource locations
    const resources = [
      { name: 'Local Food Bank', position: { lat: 37.7749, lng: -122.4194 } },
      { name: 'Community Kitchen', position: { lat: 37.7849, lng: -122.4094 } },
    ];
  
    // Initialize Google Map
    const initMap = (userLocation) => {
      // If userLocation is provided, center the map there, otherwise default to San Francisco
      const center = userLocation || { lat: 37.7749, lng: -122.4194 };
  
      map = new google.maps.Map(document.getElementById('map'), {
        center,
        zoom: 13,
        styles,
      });
  
      // Add markers for each resource
      resources.forEach((resource) => {
        const marker = new google.maps.Marker({
          position: resource.position,
          map,
          title: resource.name,
        });
  
        // Show an alert on click
        marker.addListener('click', () => {
          alert(`Clicked on: ${resource.name}`);
        });
      });
  
      // Add a marker for the user's location
      if (userLocation) {
        new google.maps.Marker({
          position: userLocation,
          map,
          title: 'Your Location',
          icon: {
            url: 'http://maps.google.com/mapfiles/ms/icons/blue-dot.png', // Blue marker for user location
          },
        });
      }
    };
  
    onMount(() => {
      // Try to get the user's current location
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const userLocation = {
              lat: position.coords.latitude,
              lng: position.coords.longitude,
            };
  
            if (!window.google) {
              // Dynamically load the Google Maps script
              const script = document.createElement('script');
              script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyCaKhk4wwQojO33N-MCBmK9lByi968scTM&callback=initMap`;
              script.defer = true;
              script.async = true;
              document.head.appendChild(script);
  
              // Attach the initMap function to the global window object
              window.initMap = () => initMap(userLocation);
            } else {
              // If script is already loaded, initialize the map immediately
              initMap(userLocation);
            }
          },
          (error) => {
            console.error('Error getting user location:', error);
            // Fall back to default center if location access is denied
            if (!window.google) {
              const script = document.createElement('script');
              script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyCaKhk4wwQojO33N-MCBmK9lByi968scTM&callback=initMap`;
              script.defer = true;
              script.async = true;
              document.head.appendChild(script);
  
              // Attach the initMap function to the global window object
              window.initMap = () => initMap(null);
            } else {
              initMap(null);
            }
          }
        );
      } else {
        console.error('Geolocation is not supported by this browser.');
        // Fall back to default center
        if (!window.google) {
          const script = document.createElement('script');
          script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyCaKhk4wwQojO33N-MCBmK9lByi968scTM&callback=initMap`;
          script.defer = true;
          script.async = true;
          document.head.appendChild(script);
  
          // Attach the initMap function to the global window object
          window.initMap = () => initMap(null);
        } else {
          initMap(null);
        }
      }
    });
  </script>
  
  <style>
    #map {
      height: 800px;
      width: 100%;
    }
  </style>
  
  <div id="map"></div>
  