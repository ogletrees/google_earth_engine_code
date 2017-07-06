
```javascript
// this gets a fusion table of your polygons
// This one is all 50k+ block groups
var fc = ee.FeatureCollection("ft:1pY83uZSHLdxybhJKSkpSVRnBjdtopP9k9nusCkyM")

// filter the set by one of the property fields. This one is 'state and we selected GA
var filteredFC = fc.filter(ee.Filter.eq('state', 'GA'));

// Count up the number of objects and print it to the console
var count = filteredFC.size();
print('Count: ', count);

// This prints the collection to the console. It has all elements there. Not ideal for large collections.
print(filteredFC)

//Make a list of the elements. Set the number to be high enough to get all of them (see count above)
var fcList = filteredFC.toList(1000)

// Print one elements to the console. Indexing starts at 1.
print(ee.Feature(ee.List(fcList).get(1)))

// get one element from the list, that was from the collection
var bg = ee.Feature(ee.List(fcList).get(100))

// Map it out, just the one element
Map.centerObject(bg);
Map.addLayer(bg)
```
