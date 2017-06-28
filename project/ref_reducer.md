```javascript
//
// Function to apply a reducer/stat to vector zones
//
function VectorZoneStats(imCollection,zoneFc,reducer,timeStampName,timeStampFormat,scale){
  //decide the scale used in reduceRegions(), which impacts the computation time significantly!!!
  if (scale === undefined){
    //use the raw scale of imCollection
    scale = ee.Image(imCollection.first()).projection().nominalScale().getInfo();
    //print('Image Collection Scale',scale);
  }
  print("VectorZoneStats() scale:", scale);
  
  // map function that reduce the regions for each image
  var fcOfFcs=imCollection.map(function(im){
      // apply the reducer to all the regions
      var fcOut = im.reduceRegions(zoneFc,reducer,scale); 
      var imStartDate = ee.Date(im.get("system:time_start")).format(timeStampFormat);
      var imEndDate = ee.Date(im.get("system:time_end")).format(timeStampFormat);
      // add image start and end time stamp to the features
      var fc = fcOut.map(function(feat) {
        return feat.set(timeStampName+"_start",imStartDate).set(timeStampName+"_end",imEndDate);
      });
      return fc;
  });
  
  // flatten the FC of FC 
  var fc=ee.FeatureCollection(fcOfFcs).flatten();
  return fc;
}
```
