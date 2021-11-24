<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div id='map-3' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map-2' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map-4' style='width: 400px; height: 300px;'></div>
    <br>
    <div id='map-5' style='width: 400px; height: 300px;'></div>
    <br>
    <div id="map-5"></div>
    <div class="map-overlay top">
      <div class="map-overlay-inner">
        <fieldset>
          <label>Select projection</label>
          <select id="projection" name="projection">
          <option value="albers">Albers</option>
          <option value="equalEarth">Equal Earth</option>
          <option value="equirectangular">Equirectangular</option>
          <option value="lambertConformalConic" selected="">
          Lambert Conformal Conic
          </option>
          <option value="mercator">Mercator</option>
          <option value="naturalEarth">Natural Earth</option>
          <option value="winkelTripel">Winkel Tripel</option>
          </select>
          </fieldset>
          <fieldset class="conic-param-input">
          <label>Center Longitude: <span id="lng-value">0</span></label>
          <input id="lng" type="range" min="-180" max="180" step="any" value="0">
          </fieldset>
          <fieldset class="conic-param-input">
          <label>Center Latitude: <span id="lat-value">30</span></label>
          <input id="lat" type="range" min="-90" max="90" step="any" value="30">
          </fieldset>
          <fieldset class="conic-param-input">
          <label>Southern Parallel Lat: <span id="lat1-value">30</span></label>
          <input id="lat1" type="range" min="-90" max="90" step="any" value="30">
          </fieldset>
          <fieldset class="conic-param-input">
          <label>Northern Parallel Lat: <span id="lat2-value">30</span></label>
          <input id="lat2" type="range" min="-90" max="90" step="any" value="30">
        </fieldset>
      </div>
    </div>
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
      this.generateMap3();
      this.generateMap4();
      this.generateMap5();
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
      },
      generateMap4: function() {
        mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const map = new mapboxgl.Map({
        container: 'map-4',
        style: 'mapbox://styles/mapbox/light-v10',
        center: [-87.62712, 41.89033],
        zoom: 15.5,
        pitch: 45
        });
        
        function rotateCamera(timestamp) {
        // clamp the rotation between 0 -360 degrees
        // Divide timestamp by 100 to slow rotation to ~10 degrees / sec
        map.rotateTo((timestamp / 100) % 360, { duration: 0 });
        // Request the next frame of the animation.
        requestAnimationFrame(rotateCamera);
        }
        
        map.on('load', () => {
        // Start the animation.
        rotateCamera(0);
        
        // Add 3d buildings and remove label layers to enhance the map
        const layers = map.getStyle().layers;
        for (const layer of layers) {
        if (layer.type === 'symbol' && layer.layout['text-field']) {
        // remove text labels
        map.removeLayer(layer.id);
        }
        }
        
        map.addLayer({
        'id': '3d-buildings',
        'source': 'composite',
        'source-layer': 'building',
        'filter': ['==', 'extrude', 'true'],
        'type': 'fill-extrusion',
        'minzoom': 15,
        'paint': {
        'fill-extrusion-color': '#aaa',
        
        // use an 'interpolate' expression to add a smooth transition effect to the
        // buildings as the user zooms in
        'fill-extrusion-height': [
        'interpolate',
        ['linear'],
        ['zoom'],
        15,
        0,
        15.05,
        ['get', 'height']
        ],
        'fill-extrusion-base': [
        'interpolate',
        ['linear'],
        ['zoom'],
        15,
        0,
        15.05,
        ['get', 'min_height']
        ],
        'fill-extrusion-opacity': 0.6
        }
        });
        });
      },
      generateMap5: function() {
        mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_KEY;
        const map = new mapboxgl.Map({
        container: 'map-5',
        style: 'mapbox://styles/mapbox/satellite-streets-v11',
        zoom: 0,
        center: [0, 1],
        projection: {
        name: 'lambertConformalConic',
        center: [0, 30],
        parallels: [30, 30]
        }
        });
        
        const projectionInput = document.getElementById('projection');
        const conicParamInputs =
        document.getElementsByClassName('conic-param-input');
        const lngInput = document.getElementById('lng');
        const lngValue = document.getElementById('lng-value');
        const latInput = document.getElementById('lat');
        const latValue = document.getElementById('lat-value');
        const lat1Input = document.getElementById('lat1');
        const lat1Value = document.getElementById('lat1-value');
        const lat2Input = document.getElementById('lat2');
        const lat2Value = document.getElementById('lat2-value');
        const inputs = [
        [lngInput, lngValue],
        [latInput, latValue],
        [lat1Input, lat1Value],
        [lat2Input, lat2Value]
        ];
        
        projectionInput.addEventListener('change', (e) => {
        const isConic = ['albers', 'lambertConformalConic'].includes(
        e.target.value
        );
        
        // Hide non-conic projection params
        for (const input of conicParamInputs) {
        input.style.display = isConic ? 'block' : 'none';
        }
        
        map.setProjection(e.target.value);
        
        if (isConic) {
        const { center, parallels } = map.getProjection();
        lngInput.value = center[0];
        latInput.value = center[1];
        lat1Input.value = parallels[0];
        lat2Input.value = parallels[1];
        }
        for (const [input, value] of inputs) {
        value.textContent = input.value;
        }
        });
        
        for (const [input, value] of inputs) {
        input.addEventListener('change', (e) => {
        value.textContent = e.target.value;
        map.setProjection({
        name: projectionInput.value,
        center: [Number(lngInput.value), Number(latInput.value)],
        parallels: [Number(lat1Input.value), Number(lat2Input.value)]
        });
        });
        }
      }
    },
  };
</script>
