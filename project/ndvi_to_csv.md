```javascript
// Get Landsat 8 collection and filter to 2015
var l8 = ee.ImageCollection('LANDSAT/LC8_L1T_TOA')
  .filterDate('2015-01-01', '2015-12-31');

// get the region from fusion table in Google Drive
var fc = ee.FeatureCollection("ft:1pY83uZSHLdxybhJKSkpSVRnBjdtopP9k9nusCkyM")

// NDVI function
var addNDVI = function(image) {
  var ndvi = image.normalizedDifference(['B5', 'B4']).rename('NDVI');
  return image.addBands(ndvi);
};


// map over collection
var withNDVI = l8.map(addNDVI);


// Make a "greenest" pixel composite.
var greenest = withNDVI.qualityMosaic('NDVI');

// Get just the NDVI value band
var val_NDVI = greenest.select('NDVI')

// Display the result.
// var visParams = {bands: ['B4', 'B3', 'B2'], max: 0.3};
Map.centerObject(fc, 9);
Map.addLayer(val_NDVI)
Map.addLayer(fc)
 
// Map over feature collection
var meanBGndvi = val_NDVI.reduceRegions({
  collection: fc,
  reducer: ee.Reducer.mean(),
  scale: 30 // 30 meters
});

// Print the first to check, just the first 6 elements
print(meanBGndvi.toList(6))

// export to google drive
Export.table.toDrive({
  collection: meanBGndvi, 
  description: 'cities_region_ndvi', 
  folder: 'RemoteSensingWork', 
  //fileNamePrefix: , 
  fileFormat: 'CSV'
  }); 






```
