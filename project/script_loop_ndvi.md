This script will get the imagery (LandSat 2015 greenest pixel NDVI) for the features that are the city block groups, dissolved, and buffered by 10km.

```javascript
// Get Landsat 8 collection and filter to 2015
var l8 = ee.ImageCollection('LANDSAT/LC8_L1T_TOA')
  .filterDate('2015-01-01', '2015-12-31');

// get the region from fusion table in Google Drive
var fc = ee.FeatureCollection("ft:1tEBkfHOZzaXQ4RzzSse7IZ5A_dGuiHXvO2xRHedx");

// var filteredFC = fc.filter(ee.Filter.eq('state', 'GA'));
// print(filteredFC)


// NDVI function //
var addNDVI = function(image) {
  var ndvi = image.normalizedDifference(['B5', 'B4']).rename('NDVI');
  return image.addBands(ndvi);
};


// map over collection
var withNDVI = l8.map(addNDVI);


// Make a "greenest" pixel composite.
var greenest = withNDVI.qualityMosaic('NDVI');
print(greenest);

// Get just the NDVI value band
var val_NDVI = greenest.select('NDVI')

// Display the result.
// var visParams = {bands: ['B4', 'B3', 'B2'], max: 0.3};
Map.centerObject(fc, 11);
Map.addLayer(val_NDVI)
 
var count = fc.size();
print('Count: ', count);

var regions = fc.toList(fc.size()).getInfo();
// print(regions);

for(var f=0; f<regions.length; f++) {
 var feature = ee.Feature(regions[f]);
 var maxNDVI = val_NDVI.clip(feature);
  Export.image.toDrive({
    image: maxNDVI, 
    description: 'Export_'+f, 
    region: feature.geometry(), 
    scale: 30
    })};
    


```
This sets up 68 tasks to download to Google Drive. Unfortunately you have to click these yourself.
