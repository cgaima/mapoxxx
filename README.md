# mapoxxx
<!DOCTYPE html>
<html>
<head>s
<meta charset=utf-8 />
<title>Geolocation</title>
<meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
<script src='https://api.mapbox.com/mapbox.js/v2.2.1/mapbox.js'></script>
<link href='https://api.mapbox.com/mapbox.js/v2.2.1/mapbox.css' rel='stylesheet' />
<script src='https://api.mapbox.com/mapbox.js/v2.2.1/mapbox.js'></script>
<link href='https://api.mapbox.com/mapbox.js/v2.2.1/mapbox.css' rel='stylesheet' />

<iframe width='100%' height='500px' frameBorder='0' src='https://a.tiles.mapbox.com/v4/cgaima.82f80d17/attribution,zoompan,zoomwheel,geocoder,share.html?access_token=pk.eyJ1IjoiY2dhaW1hIiwiYSI6IjFiM2E1NzQyYTU5YmVjYTBhMTgxOTI3ZDgyM2ViOGEwIn0.b2NXYMVkV72BjFDq8pMJjg'></iframe>

<style>
  body { margin:0; padding:0; }
  #map { position:absolute; top:0; bottom:0; width:100%; }
</style>
</head>
<body>
<style>
.ui-button {
  background:#3887BE;
  color:#FFF;
  display:block;
  position:absolute;
  top:50%;left:50%;
  width:160px;
  margin:-20px 0 0 -80px;
  z-index:100;
  text-align:center;
  padding:10px;
  border:1px solid rgba(0,0,0,0.4);
  border-radius:3px;
  }
  .ui-button:hover {
    background:#3074a4;
    color:#fff;
    }
</style>

<div id='map'></div>
<a href='#' id='geolocate' class='ui-button'>Find me</a>

<script>
L.mapbox.accessToken = 'pk.eyJ1IjoiY2dhaW1hIiwiYSI6IjFiM2E1NzQyYTU5YmVjYTBhMTgxOTI3ZDgyM2ViOGEwIn0.b2NXYMVkV72BjFDq8pMJjg';
var geolocate = document.getElementById('geolocate');
var map = L.mapbox.map('map', 'mapbox.streets');

var myLayer = L.mapbox.featureLayer().addTo(map);

// This uses the HTML5 geolocation API, which is available on
// most mobile browsers and modern browsers, but not in Internet Explorer
//
// See this chart of compatibility for details:
// http://caniuse.com/#feat=geolocation
if (!navigator.geolocation) {
    geolocate.innerHTML = 'Geolocation is not available';
} else {
    geolocate.onclick = function (e) {
        e.preventDefault();
        e.stopPropagation();
        map.locate();
    };
}

// Once we've got a position, zoom and center the map
// on it, and add a single marker.
map.on('locationfound', function(e) {
    map.fitBounds(e.bounds);

    myLayer.setGeoJSON({
        type: 'Feature',
        geometry: {
            type: 'Point',
            coordinates: [e.latlng.lng, e.latlng.lat]
        },
        properties: {
            'title': 'Here I am!',
            'marker-color': '#ff8888',
            'marker-symbol': 'star'
        }
    });

    // And hide the geolocation button
    geolocate.parentNode.removeChild(geolocate);
});

// If the user chooses not to allow their location
// to be shared, display an error message.
map.on('locationerror', function() {
    geolocate.innerHTML = 'Position could not be found';
});
</script>
</body>
</html>
