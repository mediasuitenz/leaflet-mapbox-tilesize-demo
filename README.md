# Leaflet Mapbox Tile size Demo

By default, Leaflet assumes that the tiles coming from a tile server are 256x256. However, Mapbox defaults to 512x512 tiles.

You can force Mapbox to return 256 tiles, but that increases the number of tiles needed to fill the map, and given that Mapbox charges on a per tile request basis, we want to make those requests as minimal as possible.

Example of 256 tile layer vs 512 configuration:
```js
// Replace these with your own if you're wanting to test a specific Mapbox style
// The token will need to be valid for the style you use, especially if it's a private style
var accessToken = 'pk.eyJ1IjoibGVjdG9yd291dGVyIiwiYSI6ImNrM25qZWs1dTB4NHgza240bW0zOG1qZngifQ.1uF5JjJA8l5SpTW3NVQJJQ'
var styleId = 'mapbox/streets-v11'
var attribution = 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>'

L.tileLayer(
    // Note the /256/ in the url, forcing Mapbox to return 256x256 tiles
    "https://api.mapbox.com/styles/v1/{id}/tiles/256/{z}/{x}/{y}?access_token={accessToken}",
    {
        id: styleId,
        accessToken,
        attribution,
    }
)

L.tileLayer(
    // @2x retrieves 1024px tiles, mapbox scales to 512 due to tileSize
    "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}@2x?access_token={accessToken}",
    {
        id: styleId,
        accessToken,
        tileSize: 512, // Let Leaflet know that the tiles are non-standard size
        zoomOffset: -1, // Needed to correct the zoom level for the larger tiles
        attribution,
    }
)
```
