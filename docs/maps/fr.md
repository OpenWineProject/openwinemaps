---
title: France
layout: default
parent: Wine Maps
---

# Viticulture Areas France
The 8 Main Wine Regions in France

1. Bordeaux
2. Burgundy (Bourgogne)
3. Champagne
4. Languedoc-Roussillon area in southeast France is the largest wine region.
5. Loire Valley (Val de Loire)
6. The Rhône Valley (Côtes du Rhône)
7. Alsace
8. Savoie and the Jura

<div id="avas" style="width: 100%; height: 400px" ></div>

<script>

    var map = L.map('avas', {
        center: [46.70, 2.70],
        zoom: 5,
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

    map.on('click', function(e) {
        console.log(e.latlng.lat,e.latlng.lng);
    });

    fetch('fr_regions.geojson')
        .then(response => response.json())
        .then(geojsonFeature => {
            var avas = L.geoJSON(geojsonFeature, {
                style: function (feature) {
                    return {color: 'purple', weight: .75};
                }
            }).bindTooltip(function (layer) {
                return "<b>Name: </b>" + layer.feature.properties.name + "<br><b>Appellation: </b>" + layer.feature.properties.appellation;
            }).addTo(map);
            
            layerControl.addOverlay(avas, "Regions").addTo(map);
        });    
</script>

## Wine Regions of France

Loire Valley
Champagne
Moselle
Alsace
Burgundy
Jura
Beaujolais
Bugey
Savoie
Rhône Valley
Provence
Languedoc
Roussillon
Corse
South West
Bordelais