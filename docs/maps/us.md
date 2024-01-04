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
        scrollWheelZoom: false,
        fullscreenControl: {
            pseudoFullscreen: true
        }
    });

    var attribution = 'Map <a href="/credits#maps">credits</a>.';

    var baseLayer = L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution
    }).addTo(map);    

    var OpenTopoMap = L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
        maxZoom: 17,
        attribution
    });  

    var Esri_WorldImagery = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
        attribution   
    });

    var Esri_WorldPhysical = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Physical_Map/MapServer/tile/{z}/{y}/{x}', {
        maxZoom: 8,
        attribution
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