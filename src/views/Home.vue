<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div id='map' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map-2' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map-3' style='width: 400px; height: 300px;'></div>
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
        places: [
	        { lat: -25.363, lng: 131.044, description: "A place in Australia" },
	        { lat: -33.8675, lng: 151.207, description: "The main city!" },
	        { lat: 41.7658, lng: 72.6734, description: "The main city!" },
	        // { lat: -115.1398, lng: 36.1699, description: "Las Vegas, NV!" },
          // { lat: 144.7937, lng: 13.4443, description: "Guam, USA!" }
	      ]
      };
    },
    mounted: function () {
      this.generateMap();
      this.generateMap2();
      this.generateMap3()
    },
    methods: {
      generateMap: function() {
        mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const monument = [-77.0353, 38.8895];
        const map = new mapboxgl.Map({
        container: 'map-3',
        style: 'mapbox://styles/mapbox/light-v10',
        center: monument,
        zoom: 12
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
            zoom: 1 // starting zoom
        });

        for (var i = 0; i < this.places.length; i++) {
          const marker1 = new mapboxgl.Marker()
          .setLngLat(this.places[i])
          .addTo(map);  
        }

        // const marker1 = new mapboxgl.Marker()
        //   .setLngLat(this.places[1])
        //   .addTo(map);
          
          // Create a default Marker, colored black, rotated 45 degrees.
          const marker2 = new mapboxgl.Marker({ color: 'black', rotation: 45 })
          .setLngLat([12.65147, 55.608166])
          .addTo(map);
        // create the popup
        const popup = new mapboxgl.Popup({ offset: 25 }).setText(
        'Construction on the Washington Monument began in 1848.'
        );

        // Add the control to the map.
        map.addControl(
        new MapboxGeocoder({
          accessToken: mapboxgl.accessToken,
          mapboxgl: mapboxgl
          })
        );
      },
      generateMap3: function() {
        mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const videoStyle = {
        'version': 8,
        'sources': {
        'satellite': {
        'type': 'raster',
        'url': 'mapbox://mapbox.satellite',
        'tileSize': 256
        },
        'video': {
        'type': 'video',
        'urls': [
        'https://static-assets.mapbox.com/mapbox-gl-js/drone.mp4',
        'https://static-assets.mapbox.com/mapbox-gl-js/drone.webm'
        ],
        'coordinates': [
        [-122.51596391201019, 37.56238816766053],
        [-122.51467645168304, 37.56410183312965],
        [-122.51309394836426, 37.563391708549425],
        [-122.51423120498657, 37.56161849366671]
        ]
        }
        },
        'layers': [
        {
        'id': 'background',
        'type': 'background',
        'paint': {
        'background-color': 'rgb(4,7,14)'
        }
        },
        {
        'id': 'satellite',
        'type': 'raster',
        'source': 'satellite'
        },
        {
        'id': 'video',
        'type': 'raster',
        'source': 'video'
        }
        ]
        };
        
        const map = new mapboxgl.Map({
        container: 'map',
        minZoom: 14,
        zoom: 17,
        center: [-122.514426, 37.562984],
        bearing: -96,
        style: videoStyle
        });
        
        let playingVideo = true;
        
        map.on('click', () => {
        playingVideo = !playingVideo;
        
        if (playingVideo) {
        map.getSource('video').play();
        } else {
        map.getSource('video').pause();
        }
        });
      }
    },
  };
</script>
