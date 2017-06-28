For my project I want to eventually get the mean NDVI value for a block group in the year 2015.  
I can calculate the 'greenest' pixel, which is the highest NDVI value for a collection of iamges (2015-01-01 to 2015-12-31)

THIS GETS THE GREENEST PIXEL OR HIGHEST NDVI SCORE FOR LANDSAT IMAGERY IN 2015

```javascript
// Get Landsat 8 collection and filter to 2015
var l8 = ee.ImageCollection('LANDSAT/LC8_L1T_TOA')
  .filterDate('2015-01-01', '2015-12-31');
  
var point = /* color: #d63000 */ee.Geometry.Point([-84.3914794921875, 33.78827853625996]);

// Get the least cloudy image in 2015. This is am image, not a collection of images.
var image = ee.Image(
  l8.filterBounds(point)
    .filterDate('2015-01-01', '2015-12-31')
    .sort('CLOUD_COVER')
    .first()
);

var addNDVI = function(image) {
  var ndvi = image.normalizedDifference(['B5', 'B4']).rename('NDVI');
  return image.addBands(ndvi);
};

// Test the addNDVI function on a single image.
// var ndvi = addNDVI(image).select('NDVI');

// Now get a collection of images and apply the funstion.
// map over collection
var withNDVI = l8.map(addNDVI);


// Make a "greenest" pixel composite.
var greenest = withNDVI.qualityMosaic('NDVI');

// Display the result.
 var visParams = {bands: ['B4', 'B3', 'B2'], max: 0.3};
 Map.centerObject(point, 9);
 Map.addLayer(greenest, visParams, 'Greenest pixel composite');
 ```
