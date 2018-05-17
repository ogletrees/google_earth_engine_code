In this case I want to extract some values from a raster to either points or polygons.
```js
// this is for using 2 reducers
var meancity = img.reduceRegions({
  collection: featurecoll,
  reducer: ee.Reducer.mean().setOutputs(['rename']).combine({
		reducer2: ee.Reducer.median().setOutputs(['rename']),
    sharedInputs: true
  }),
  scale: 30 // 30 meters for Landsat imagery
});

```
and I probably want to save that out to Google Drive
```js
// drop .geo
var eviOut = meancity.select(['.*'], null, false);

// export to google drive
 Export.table.toDrive({
  collection: eviOut, 
  description: 'toa_city15_evi_mm_20180318', 
  folder: 'RemoteSensingWork', 
//fileNamePrefix: , 
  fileFormat: 'CSV'
  }); 

```
