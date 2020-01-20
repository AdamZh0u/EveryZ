# Gallery
## dens_25
``` javascript
var sh = ee.Image("users/zhouzz400/shanghai_GAIA");
print(sh);

var Viz = {min: 1, max: 34, palette: ['FFFFFF', 'FF0000']};
Map.addLayer(sh,Viz)

// get special year

    // 0-null 1-2018 34-1985
var sh_10 = sh.gte(9)

//var Viz2 = {min: 0, max: 1, palette: ['FFFFFF', 'FF0000']};
//Map.addLayer(sh_10,Viz2)

// kernal stats
var ker_sq = ee.Kernel.square({
  radius: 3, units: 'pixels', normalize: false
});
    // ee.Kernel.circle(7)
var ker_st = sh_10.reduceNeighborhood({
  reducer: ee.Reducer.mean(),
  kernel: ker_sq,
});

//var Viz3 = {min: 0, max: 1, palette: ['FFFFFF', 'FF0000']};
//var Viz4 = {min: 0, max: 1, palette: ['000000', 'FFFFFF']};
//Map.addLayer(ker_st,Viz4)
//print(ker_st)

//
var des_25 = ker_st.gte(0.25).selfMask().rename('Dens_25');
//Map.addLayer(des_25,Viz4)
//print(des_25)

Map.addLayer(des_25, {palette: 'FF0000'});
print(des_25)

var objectId = des_25.connectedComponents({
  connectedness: ee.Kernel.plus(1),
  maxSize: 256
});
Map.addLayer(objectId.randomVisualizer(), null, 'Objects');

var des_25_v = des_25.reduceToVectors({
  scale: 80,
  geometryType: 'polygon',
  eightConnected: false,
  labelProperty: 'zone',}
  );
Map.addLayer(des_25_v);

```

## search
```javascript
var point = /* color: #98ff00 */ee.Geometry.Point([114.3362584771894, 30.54952805541824]),
    l8 = ee.ImageCollection("LANDSAT/LC08/C01/T1_TOA"),
    bare = /* color: #c24823 */ee.Geometry.Polygon(
        [[[114.30517811719619, 30.554663336996253],
          [114.30161614362441, 30.552224189574872],
          [114.30958338525011, 30.55368954007891],
          [114.30803843285753, 30.5546134528199]]]),
    veget = /* color: #ff0000 */ee.Geometry.Polygon(
        [[[114.48716604174274, 30.507213819178254],
          [114.4845928059624, 30.5054401948097],
          [114.48682294356126, 30.505144587441116],
          [114.4883667488358, 30.505144633458844],
          [114.49162631694242, 30.504848979348527],
          [114.49368490531106, 30.506622614826078]]]),
    water = /* color: #00ff00 */ee.Geometry.Polygon(
        [[[114.28774101355862, 30.565245523015815],
          [114.28482277015041, 30.561845853255953],
          [114.28516609290432, 30.5602198821312],
          [114.28774101355862, 30.559480795340228],
          [114.29237587073635, 30.563619608862606]]]);

var bands = ["B2","B3","B4","B5","B6","B7"];
var image = ee.Image(l8
  .filterBounds(point)
  .sort("CLOUD_COVER")
  .first())
  .select(bands);

Map.addLayer(image,{bands:["B4","B3","B2"],max:0.3},"image");

var bareMean = image.reduceRegion({
  reducer:ee.Reducer.mean(),
  geometry:bare,
  scale:30,
  }).values();
  
var vegetMean = image.reduceRegion({
  reducer:ee.Reducer.mean(),
  geometry:veget,
  scale:30,
  }).values();
  
var waterMean = image.reduceRegion({
  reducer:ee.Reducer.mean(),
  geometry:water,
  scale:30,
  }).values();
  
var chart = ui.Chart.image.regions(image,ee.FeatureCollection([
  ee.Feature(bare, {label:"bare"}),
  ee.Feature(veget,{label:"vaget"}),
  ee.Feature(water,{label:"water"})]),
  ee.Reducer.mean(),30,"label",[0.48,0.56,0.65,0.86,1.61,2.2]
);
print(chart);

var endmembers = ee.Array.cat([bareMean,vegetMean,waterMean],1);
var arrayImage = image.toArray().toArray(1);
var unmixed = ee.Image(endmembers).matrixSolve(arrayImage);
var unmixedImage = unmixed.arrayProject([0])
                          .arrayFlatten([["bare","veget","water"]]);
Map.addLayer(unmixedImage,{},"fractions")
```

