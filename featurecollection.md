```javascript
// this gets a fusion table of your polygons
var fusionTableFC = ee.FeatureCollection("ft:1MUjV9wNtbEyyHX9l-SP50KF00loG1nB0_evRut10")

// this will print the object
print (fusionTableFC)

// this will center the map on the feature collection
Map.centerObject(fusionTableFC);

// this also displays it on the map
Map.addLayer(fusionTableFC)

// This will show the count of features in the collection
print('Count: ', fusionTableFC.size());

// This just print the attributes of the one item. This just limits the number we access
// print(fusionTableFC.limit(1));

// This get the first 6 features to a list
var flist = fusionTableFC.toList(6)

// and print the list to the console
print(flist)

// to pull one feature from the collection you can filter it by a property ('Name' in this case)
// var filteredFC = fusionTableFC.filter(ee.Filter.eq('Name', 'Greensboro'));
// print (filteredFC)
```
