## function compute area
``` javascript
var Cities = ee.FeatureCollection("users/zhouzz400/Boundries/China_Cities")
print(Cities);

function Add_Area(feature){
  var the_Area = ee.Number(feature.area())
  return feature.set("Area_km2",the_Area.divide(1000*1000))
}

var City_with_Area = Cities.map(Add_Area);

print(Cities.first(),City_with_Area.first());
```
## function compute NDVI
``` javascript
var L8 = ee.ImageCollection("LANDSAT/LC08/C01/T1_TOA")
  .filterBounds(ee.Geometry.Point(107.193,29.1373))
  .filterDate("2019-01-01","2020-01-01")
  .select("B[4,5]")
  .limit(3);
  
function add_NDVI(image){
  var NDVI = image.normalizedDifference(["B5","B4"]);
  return image.addBands(NDVI);
}

var L8_NDVI = L8.map(add_NDVI);

print(L8.first(),L8_NDVI.first());
Map.addLayer(L8_NDVI.select("nd"));
Map.addLayer(L8.limit(1).select("B[4,5]").mean());
```