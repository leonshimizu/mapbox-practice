<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div id='map' style='width: 400px; height: 300px;'></div>
  </div>
</template>

<style></style>

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
    },
    methods: {
      generateMap: function() {
          mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const map = new mapboxgl.Map({
            container: 'map', // container ID
            style: 'mapbox://styles/mapbox/streets-v11', // style URL
            center: [12.5, 55.7], // starting position [lng, lat]
            zoom: 9 // starting zoom
        });
        const marker1 = new mapboxgl.Marker()
          .setLngLat([12.554729, 55.70651])
          .addTo(map);
          
          // Create a default Marker, colored black, rotated 45 degrees.
          const marker2 = new mapboxgl.Marker({ color: 'black', rotation: 45 })
          .setLngLat([12.65147, 55.608166])
          .addTo(map);

      }
    },
  };
</script>
