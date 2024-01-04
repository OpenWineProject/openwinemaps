---
title: United States    
layout: default
parent: Wine Maps
---

# United Sates

The United States officially recognizes 259 regulated American Viticulture Areas (AVA's). 

## Viticulture Areas

<div id="avas" style="width: 100%; height: 400px" ></div>

<script>

    var map = L.map('avas', {
        center: [40.02, -100.88],
        zoom: 4,
        minZoom: 1
    });

    var baseLayer = L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '© OpenStreetMap contributors, Tiles style by Humanitarian OpenStreetMap Team hosted by OpenStreetMap France'
    }).addTo(map);    

    var OpenTopoMap = L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
        maxZoom: 17,
        attribution: 'Map data: &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)'
    });  

    var Esri_WorldImagery = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
    });

    var Esri_WorldPhysical = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Physical_Map/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Source: US National Park Service',
        maxZoom: 8
    });    

    var baseMaps = {
        "Default": baseLayer,
        "Topology": OpenTopoMap,
        "Sattelite": Esri_WorldImagery,
        "Terrain": Esri_WorldPhysical
    };

    var layerControl = L.control.layers(baseMaps).addTo(map);

    fetch('us-avas.geojson')
        .then(response => response.json())
        .then(geojsonFeature => {
            var avas = L.geoJSON(geojsonFeature, {
                style: function (feature) {
                    return {color: 'purple', weight: .75};
                }
            }).bindTooltip(function (layer) {
                return "<b>Name: </b>" + layer.feature.properties.name + "<br><b>Date Created: </b>" + layer.feature.properties.created;
            }).addTo(map);
            
            layerControl.addOverlay(avas, "AVA's").addTo(map);
        });

</script>