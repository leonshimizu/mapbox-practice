<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div id='map' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map-2' style='width: 400px; height: 300px;'></div>
  </div>
</template>

<style>
  #marker {
  background-image: url('https://docs.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg');
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
  }
  
  .mapboxgl-popup {
  max-width: 200px;
  }
</style>

<script>
  import mapboxgl from 'mapbox-gl'; // or "const mapboxgl = require('mapbox-gl');"

  export default {
    data: function () {
      return {
        message: "Welcome to Vue.js!",
      };
    },
    mounted: function () {
      this.generateMap();
      this.generateMap2();
    },
    methods: {
      generateMap: function() {
        mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const monument = [-77.0353, 38.8895];
        const map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/light-v10',
        center: monument,
        zoom: 9
        });

        // create the popup
        const popup = new mapboxgl.Popup({ offset: 25 }).setText(
        'Construction on the Washington Monument began in 1848.'
        );

        // create DOM element for the marker
        const el = document.createElement('div');
        el.id = 'marker';

        const marker1 = new mapboxgl.Marker()
        .setLngLat([-77.036, 38.88])
        .addTo(map);
        
        // create the marker
        new mapboxgl.Marker(el)
        .setLngLat(monument)
        .setPopup(popup) // sets a popup on this marker
        .addTo(map);
      },
      generateMap2: function() {
          mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const map = new mapboxgl.Map({
            container: 'map-2', // container ID
            style: 'mapbox://styles/mapbox/streets-v11', // style URL
            center: [12.550343, 55.665957], // starting position [lng, lat]
            zoom: 9 // starting zoom
        });
        const marker1 = new mapboxgl.Marker()
          .setLngLat([12.554729, 55.70651])
          .addTo(map);
          
          // Create a default Marker, colored black, rotated 45 degrees.
          const marker2 = new mapboxgl.Marker({ color: 'black', rotation: 45 })
          .setLngLat([12.65147, 55.608166])
          .addTo(map);
        // create the popup
        const popup = new mapboxgl.Popup({ offset: 25 }).setText(
        'Construction on the Washington Monument began in 1848.'
        );
      }
    },
  };
</script>
