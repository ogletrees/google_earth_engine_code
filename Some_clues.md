```javascript
// Define a raster and polygon.
var image = ee.Image('LANDSAT/LC8_L1T/LC80440342014077LGN00').select('B[2-5]');
var region = ee.Geometry.Rectangle(-122.45, 37.74, -122.4, 37.8);

// Run histogram on raster, on region defined by polygon.
var tryit = ee.Dictionary(
  image.reduceRegion(ee.Reducer.frequencyHistogram(), region, 30).get("B2")
);
print('tryit', tryit);

// Extract the cell count associated with a cell value (10001) 
print('cell count for value 10001', tryit.get('10001'));

//#—————————————————-
//Another script from Stackoverflow
//#------------------
var landsat8= ee.ImageCollection('LANDSAT/LC8_L1T_TOA').filterBounds(geometry)
var waterThreshold = 0; 

// water function:
var waterfunction = function(image){
  //add the NDWI band to the image
  var ndwi = image.normalizedDifference(['B3', 'B6']).rename('NDWI');
  //get pixels above the threshold
  var water01 = ndwi.gt(waterThreshold);
  //mask those pixels from the image
  image = image.updateMask(water01).addBands(ndwi);

  var area = ee.Image.pixelArea();
  var waterArea = water01.multiply(area).rename('waterArea');

  image = image.addBands(waterArea);

  var stats = waterArea.reduceRegion({
    reducer: ee.Reducer.sum(), 
    geometry: geometry, 
    scale: 30,
  });

  return image.set(stats);
};
var collection = landsat8.map(waterfunction);
print(collection);

var chart = ui.Chart.image.series({
  imageCollection: collection.select('waterArea'), 
  region: geometry, 
  reducer: ee.Reducer.sum(), 
  scale: 30,
});
print(chart);
```
