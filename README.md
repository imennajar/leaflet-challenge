# leaflet-challenge
The USGS provides earthquake data in a number of different formats, updated every 5 minutes. In this project, we will visualize earthquake data from all over the world each day,for the past 7 days and developing a way to better educate the public and other government organizations on issues facing our planet.


Link from where we will get our data: https://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php

## What we will learn from this project:

- How to interact between HTML,CSS and JavaScript code

- How to retrieve data from a GeoJson file (provided in URL link: https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson)

- How to use Leaflet to create a map from the dataset

-

## Instructions: 

### Part1:
 
  - Use the D3 library to get the dataset from the URL provided,

  - Use Leaflet to create a map that plots all the earthquakes from the dataset based on their longitude and latitude and the data markers should reflect the magnitude of the earthquake by their size and the depth of the earthquake by color.

  - Include popups that provide additional information about the earthquake when its associated marker is clicked,

  - Create a legend that will provide context for your map data.

### Prt2:

- Plot the tectonic plates dataset on the map in addition to the earthquakes,

- Add other base maps to choose from,

- Put each dataset into separate overlays that can be turned on and off independently,

- Add layer controls to your map.

 ## Program:

### Tools:

- Visual Studio Code (VSCode): is a free, open-source code editor developed by Microsoft.

- Javascript: is a programming language primarily used for web development.

- CSS: is a stylesheet language used for describing the presentation and formatting of documents written in HTML.

- HTML: is the standard markup language for documents designed to be displayed in a web browser.

- D3.js:  is a JavaScript library for manipulating and visualizing data in web browsers.

- Leaflet: is an open-source JavaScript library for interactive maps.

- OpenStreetMap: is a mapping project for creating and providing free geographic data and mapping.

### Code:

#### Part1:

##### HTML: index.html

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Leaflet Earthquake</title>

  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
    crossorigin="" />

  <!-- Our CSS -->
  <link rel="stylesheet" type="text/css" href="static/css/style.css">
</head>

<body>

  <!-- The div that holds our map -->
  <div id="map" class="grayscale"></div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
    integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
    crossorigin=""></script>
  <!-- D3 JavaScript -->
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <!-- Our JavaScript -->
  <script type="text/javascript" src="static/js/logic.js"></script>
</body>

</html>
```

##### CSS: style.css

```
body {
  padding: 0;
  margin: 0;
}

#map,
body,
html {
  height: 100%;
}

/* Apply grayscale to map tiles */
.grayscale .leaflet-tile {
  filter: grayscale(70%);
}

/* Updated CSS for legend below */
.info {
  background-color: white;
  font-size: 12px;
  padding: 10px;
  border: 5px solid #ccc;
}

/* Add background color for the legend colors */
.legend-color {
  width: 24px;
  height: 24px;
  display: inline-block;
  margin-right: 5px;
}
```
##### JavaScript: logic.js

```
// Initialize the map
let myMap = L.map("map", {
    center: [37.0902, -95.7129],
    zoom: 5
});

// Add a tile layer
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
}).addTo(myMap);

// Define the URL for earthquake data
let url = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson";

// Define a function for color based on depth
function getColor(depth) {
    if (depth <= 10) {
        return 'yellow'; 
    } else if (depth <= 30) {
        return 'green'; 
    } else if (depth <= 50) {
        return 'orange'; 
    } else if (depth <= 70) {
        return 'red'; 
    } else if (depth <= 90) {
        return 'darkred'; 
    } else {
        return 'black'; 
    }
}

// Create the legend
var legend = L.control({ position: 'bottomright' });
// Define the legend and create a new HTML <div>
legend.onAdd = function (map) {
    var div = L.DomUtil.create('div', 'info legend');
    var grades = [-10, 10, 30, 50, 70, 90];
    // Create labels with colors
    for (var i = 0; i < grades.length; i++) {
        div.innerHTML +=
            '<i class="legend-color" style="background-color:' + getColor(grades[i] + 1) + '"></i> ' +
            grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
    }

    return div;
};
// Add the legend to the map
legend.addTo(myMap);

// Use D3.js to load earthquake data and create markers
d3.json(url).then(function (response) {
    response.features.forEach(function (quake) {
        let magnitude = quake.properties.mag;
        let coordinates = quake.geometry.coordinates;
        let place = quake.properties.place;
        let depth = quake.geometry.coordinates[2];
        let time = quake.properties.time; 
        date = new Date(time);
        // Create a circle marker
        let marker = L.circleMarker([coordinates[1], coordinates[0]], {
            radius: magnitude * 3,
            fillColor: getColor(depth),
            color: 'gray',
            weight: 1,
            opacity: 0.8,
            fillOpacity: 0.35
            });

             // Create the popup  with information
            let popup = `<strong>Magnitude:</strong> ${magnitude}<br><strong>Place:</strong> ${place}<br><strong>Depth:</strong> ${depth} km<br><strong>Time:</strong> ${date}`;

            // Add the popup
            marker.bindPopup(popup).addTo(myMap);
        
    });
});

```

##### Map:
<img src='map1.png' style ='width:700px;height:300px'/> 

#### Part2:

##### HTML: index.html

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Earthquake & Tectonic Plates</title>

  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
    crossorigin="" />

  <!-- Our CSS -->
  <link rel="stylesheet" type="text/css" href="static/css/style.css">
</head>

<body>

  <!-- The div that holds our map -->
  <div id="map"></div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
    integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
    crossorigin=""></script>
  <!-- D3 JavaScript -->
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <!-- Our JavaScript -->
  <script type="text/javascript" src="static/js/logic.js"></script>
</body>

</html>
```

##### CSS: style.css

```
body {
  padding: 0;
  margin: 0;
}

#map,
body,
html {
  height: 100%;
}

/* Updated CSS for legend below */
.info {
  background-color: white;
  font-size: 12px;
  padding: 10px;
  border: 5px solid #ccc;
}

.legend-title {
  font-weight: bold;
  margin-bottom: 5px;
}
/* Add background color for the legend colors */
.legend-color {
  width: 24px;
  height: 24px;
  display: inline-block;
  margin-right: 5px;
}
```
##### JavaScript: logic.js

```
// Define tile layers
let osmLayer = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
});

let satelliteLayer = L.tileLayer('https://{s}.google.com/vt/lyrs=s&x={x}&y={y}&z={z}', {
    maxZoom: 30,
    subdomains: ['mt0', 'mt1', 'mt2', 'mt3'],
    attribution: '&copy; <a href="https://www.google.com/maps">Google Maps</a>'
});

let outdoorLayer = L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
    maxZoom: 17,
    attribution: '&copy; <a href="https://www.opentopomap.org/copyright">OpenTopoMap</a> contributors'
});



// Create a base layer group with all base maps
let baseLayers = {
    "OpenStreetMap": osmLayer,
    "Satellite": satelliteLayer,
    "Outdoor": outdoorLayer,
    
};

// Initialize the map with osmLayer
let myMap = L.map("map", {
    center: [37.0902, -95.7129],
    zoom: 4,
    layers: [osmLayer] // Set the default base map
});

// Create separate layer groups for earthquake and tectonic data
let earthquakeLayer = L.layerGroup().addTo(myMap);
let tectonicLayer = L.layerGroup().addTo(myMap);

// Define the URL for earthquake data
let earthquakeUrl = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson";

// Define a function for color based on depth
function getColor(depth) {
    if (depth <= 10) {
        return 'yellow'; 
    } else if (depth <= 30) {
        return 'green'; 
    } else if (depth <= 50) {
        return 'orange'; 
    } else if (depth <= 70) {
        return 'red'; 
    } else if (depth <= 90) {
        return 'darkred'; 
    } else {
        return 'black'; 
    }
}
// Create the legend
let legend = L.control({ position: 'bottomright' });
// Define the legend and create a new HTML <div>
legend.onAdd = function (map) {
    let div = L.DomUtil.create('div', 'info legend');
    let grades = [-10, 10, 30, 50, 70, 90];
    // Create labels with colors
    for (var i = 0; i < grades.length; i++) {
        div.innerHTML +=
            '<i class="legend-color" style="background-color:' + getColor(grades[i] + 1) + '"></i> ' +
            grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
    }

    return div;
};
// Add the legend to the map
legend.addTo(myMap);

// Use D3.js to load earthquake data and create markers
d3.json(earthquakeUrl).then(function (response) {
    response.features.forEach(function (quake) {

        let magnitude = quake.properties.mag;
        let coordinates = quake.geometry.coordinates;
        let depth = quake.geometry.coordinates[2];
        let place = quake.properties.place;
        let time = quake.properties.time;
        date = new Date(time);

        // Create circle marker
        let marker = L.circleMarker([coordinates[1], coordinates[0]], {
            radius: magnitude *3,
            fillColor: getColor(depth), 
            color: 'gray',
            weight: 1,
            opacity: 0.8,
            fillOpacity: 0.8
        });

        // Create the popup  with information
        let popup = `<strong>Magnitude:</strong> ${magnitude}<br><strong>Place:</strong> ${place}<br><strong>Depth:</strong> ${depth} km<br><strong>Time:</strong> ${date}`;

        // Add the popup 
        marker.bindPopup(popup).addTo(earthquakeLayer);
    });

    
});

// Define the URL for tectonic plates data
let tectonicUrl = "static/tectonic.json";

// Load tectonic plates data and add it to the tectonicLayer
d3.json(tectonicUrl).then(function (tectonicData) {
    L.geoJSON(tectonicData, {
        style: function (feature) {
            return {
                color: "red", 
                weight: 2,     
            };
        },
    }).addTo(tectonicLayer);
});

// Create a control layer
let overlays = {
    "Earthquake Data": earthquakeLayer,
    "Tectonic Plates": tectonicLayer
};

// Add control layers for base layers and overlays
L.control.layers(baseLayers, overlays, { collapsed: false }).addTo(myMap);
```
##### Map:
<img src='map2.png' style ='width:700px;height:300px'/> 


