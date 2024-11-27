<script>
  import { onMount } from 'svelte';

  let map;

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

  // Food resource locations
  const resources = [
    { name: 'Local Food Bank', position: { lat: 37.7749, lng: -122.4194 }, status: 'Open' },
    { name: 'Community Kitchen', position: { lat: 37.7849, lng: -122.4094 }, status: 'Limited Stock' },
    { name: 'Homeless Shelter', position: { lat: 37.7949, lng: -122.4294 }, status: 'Closed' },
  ];

  const initMap = (userLocation) => {
    const center = userLocation || { lat: 37.7749, lng: -122.4194 };

    // Initialize the Google Map
    map = new google.maps.Map(document.getElementById('map'), {
      center,
      zoom: 13,
      styles,
    });

    // Add markers for resources
    resources.forEach((resource) => {
      const marker = new google.maps.Marker({
        position: resource.position,
        map,
        title: resource.name,
      });

      // Add a click listener to the marker
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
          url: 'http://maps.google.com/mapfiles/ms/icons/blue-dot.png',
        },
      });
    }
  };

  onMount(() => {
    // Try to get the user's location
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

            window.initMap = () => initMap(userLocation);
          } else {
            initMap(userLocation);
          }
        },
        (error) => {
          console.error('Error getting user location:', error);
          initFallbackMap();
        }
      );
    } else {
      console.error('Geolocation is not supported by this browser.');
      initFallbackMap();
    }
  });

  const initFallbackMap = () => {
    if (!window.google) {
      const script = document.createElement('script');
      script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyCaKhk4wwQojO33N-MCBmK9lByi968scTM&callback=initMap`;
      script.defer = true;
      script.async = true;
      document.head.appendChild(script);

      window.initMap = () => initMap(null);
    } else {
      initMap(null);
    }
  };

  // Exported function to center the map on a specific location
  export function centerOnLocation(location) {
    console.log('Centering on location:', location);
    if (map && location) {
      map.setCenter(location);
      map.setZoom(15); // Zoom closer to the location
    } else {
      console.error("Map is not initialized or invalid location provided.");
    }
  }
</script>

<style>
  #map {
    height: 800px;
    width: 100%;
  }
</style>

<div id="map"></div>
