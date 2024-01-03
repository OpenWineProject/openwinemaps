---
title: United States    
layout: default
parent: Wine Maps
---

# United Sates

The United States officially recognizes 259 regulated American Viticulture Areas (AVA's). 

## Viticulture Areas

<div id="avas" style="width: 100%; height: 300px" ></div>

<script>
    var map = L.map('avas', {
        center: [40.02, -100.88],
        zoom: 4
        });
    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    var avas = fetch('us-avas.geojson')
        .then(response => response.json())
        .then(geojsonFeature => {
            L.geoJSON(geojsonFeature, {
                style: function (feature) {
                    return {color: 'purple', weight: .75};
                }
            }).bindTooltip(function (layer) {
                return "<b>Name: </b>" + layer.feature.properties.name + "<br><b>Date Created: </b>" + layer.feature.properties.created;
            }).addTo(map);
        })

</script>