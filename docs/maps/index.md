---
title: Wine Maps
layout: default
nav_order: 2
has_children: true
---

# Wine Maps

A complete artifact package should include shapfile, KML and geojson data. The `geojson` format is standard used throughout this project for its inherent versatility. 

Every effort will be made to use existing standards where possible. Otherwise we will promote already proposed specifications or propose our own.

<!-- `usa_{ava-name}.geojson` or `fr_{}` -->

## Wine Producing Regions

In 2014, the five largest producers of wine in the world were, in order, Italy, Spain, France, the United States, and China. — [Wikipedia](https://en.wikipedia.org/wiki/List_of_wine-producing_regions)

{% for country in site.data.countries %}
country.CountryName
{% endfor %}

<div id="world" style="width: 100%; height: 400px" ></div>
Choropleth map of wine producing countries by production.

<script>

    var map = L.map('world', {
        center: [40.737, -73.923],
        zoom: 2,
        scrollWheelZoom: false
    });

    var attribution = 'Map <a href="/credits#maps">credits</a>.';

    var baseLayer = L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution
    }).addTo(map); 

    async function getProductionByCountry() {
        const response = await fetch('/data/production-by-country.json');
        const producers = await response.json();
        return producers;
    }
var producers = getProductionByCountry();
console.log(producers);
    fetch('https://raw.githubusercontent.com/OpenWineProject/openwinemaps/main/docs/data/countries.geojson')
        .then(response => response.json())
        .then(countries => {

            fetch('/data/production-by-country.json')
                .then(response => response.json())
                .then(producers => producers.map(producer => {
                    
                        


                    var ranking = {
                        production: {
                            rank: producer.Rank,
                            quantity: producer.ProductionInTonnes
                        }
                    };

                    
                    var country = countries.features.findIndex(country => country.properties.ADMIN === producer.CountryName);
                    
                    /**
                     * Some items' `undefined` country properties broke this assignment.
                     * TODO: either correct the data or this assignment task.
                     * Checking for defined country.
                     */
                    if(countries.features[country]) {
                        Object.assign(countries.features[country].properties, ranking); 
                    }
                }));



            L.geoJSON(countries, {
                style: function (feature) {
                    return { 
                        weight: .75,
                        
                    };
                }
            }).addTo(map);

        });

        function getColor(d) {
            console.log(d);
            return d > 1000 ? '#800026' :
                    d > 500  ? '#BD0026' :
                    d > 200  ? '#E31A1C' :
                    d > 100  ? '#FC4E2A' :
                    d > 50   ? '#FD8D3C' :
                    d > 20   ? '#FEB24C' :
                    d > 10   ? '#FED976' :
                            '#FFEDA0';
        }
            // https://colorbrewer2.org/#type=sequential&scheme=RdPu&n=9
            // ['#fff7f3','#fde0dd','#fcc5c0','#fa9fb5','#f768a1','#dd3497','#ae017e','#7a0177','#49006a']
</script>