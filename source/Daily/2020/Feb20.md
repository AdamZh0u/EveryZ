## 19.2.2020

奶嘴
### 下载GAIA
```python
import requests
import re
url = 'http://data.ess.tsinghua.edu.cn/data/GAIA/GAIA_1985_2018_00_008.tif'
r = requests.get(url, allow_redirects=True)
#if r.headers.get( 'Content-Type')== 'Content-Type':
r.content

f = open("demo.txt")
line = f.read()
f.close()

pattern = re.compile("GAIA_1985_2018(_\-?\d{2})(_\-?\d{3}).tif")  
result = pattern.findall(line)

for i in result:
    url = "http://data.ess.tsinghua.edu.cn/data/GAIA/GAIA_1985_2018"+i[0]+i[1]+".tif"
    r = requests.get(url, allow_redirects=True)
    name = "GAIA_1985_2018"+i[0]+i[1]+".tif"
    open("../GAIA_Data/"+name, 'wb').write(r.content)
```

### 如何上传到gee
```python
!pip install --upgrade google-cloud-storage
!gsutil ls gs://gaia-zzz/

project_id = 'groovy-bay-266911'
import uuid
bucket_name = 'colab-sample-bucket-' + str(uuid.uuid1())
from google.colab import auth
auth.authenticate_user()

!gcloud config set project {project_id}

## test
with open('/tmp/to_upload_-01.txt', 'w') as f:
  f.write('my sample file')
print('/tmp/to_upload.txt contains:')
!cat /tmp/to_upload_-01.txt
!gsutil cp /tmp/to_upload_-01.txt gs://gaia-zzz/

lats = []
lats.append("%02d"% 0 )
for i in range(1,80):
    lats.append("%02d" % i)
    lats.append("%03d" % -i)
for lat in lats:
    AssetID = 'users/zhouzz400/GAIA_2018_lat/GAIA_1985_2018_'+lat
    ImageFile = 'gs://gaia-zzz/GAIA_1985_2018_' + lat + '.tif'
    #print(ImageFile,AssetID)
    line = "earthengine --no-use_cloud_api upload image --asset_id={AssetID} --nodata_value=255 {ImageFile}".format(AssetID=AssetID,ImageFile=ImageFile)
    print(line)
    !eval {line}
```
### gee 投影
 gee 使用geotools 库 不支持Interrupted_Goode_Homolosine
 可以使用mollwide
https://gis.stackexchange.com/questions/272818/google-earth-engine-reprojection-to-non-epsg-defined-crs

https://spatialreference.org/ref/sr-org/7619
 python接口会支持吗？

```Javascript
var region = "SPA"
var boun = ee.FeatureCollection("users/zhouzz400/Boundries/worldRegion")
  .filter(ee.Filter.eq("Abbrv",region)).geometry()

var GAIA = ee.ImageCollection("users/zhouzz400/GAIA")
  .filterBounds(boun).mosaic().clip(boun)
var GAIA_year = GAIA.gte(4)

var GAIA_viz = {min:0,max:34,palette:["000000","ff0000"]}
//Map.addLayer(GAIA,GAIA_viz)

function getArea(image,boun){
  var area_imag = image.multiply(ee.Image.pixelArea())
  var sumarea = ee.Number(area_imag.reduceRegion(
                  {"reducer": ee.Reducer.sum(),
                  "scale": 30,
                  "geometry":boun
                  })
                  .get("b1") )
  return sumarea}
var area = getArea(GAIA_year,boun)

//Lambert cylindrical projection epsg:9843
// WKT string
var wkt = ' \
  PROJCS["World_Mollweide", \
    GEOGCS["GCS_WGS_1984", \
      DATUM["WGS_1984", \
        SPHEROID["WGS_1984",6378137,298.257223563]], \
      PRIMEM["Greenwich",0], \
      UNIT["Degree",0.017453292519943295]], \
    PROJECTION["Mollweide"], \
    PARAMETER["False_Easting",0], \
    PARAMETER["False_Northing",0], \
    PARAMETER["Central_Meridian",0], \
    UNIT["Meter",1], \
    AUTHORITY["EPSG","54009"]]';

var proj_mollweide = ee.Projection(wkt);
var boun_moll = boun.transform(proj_mollweide,
  ee.ErrorMargin(10))
print(boun_moll.area(ee.ErrorMargin(1000)))//5010868555175.95
print(boun.area(ee.ErrorMargin(1000)))//5010868555175.796
print(boun.area(ee.ErrorMargin(10),proj_mollweide))//5022090468716.392

```
### 矢量与栅格总面积是不是相等的？
```Javascript
var boun2 = ee.FeatureCollection("users/zhouzz400/Boundries/China_Provinces")
  .filter(ee.Filter.eq("Name","湖北省")).geometry()

var GAIA = ee.ImageCollection("users/zhouzz400/GAIA")
  .filterBounds(boun2)

Map.addLayer(GAIA.mosaic().clip(boun2))
Map.addLayer(boun2)
print("mosaic area:",getArea(GAIA.mosaic())) //185940066188.5224

function getArea(image){
  var a = ee.Image(image).gte(0).clip(boun2)
  var area_imag = a.multiply(ee.Image.pixelArea())
  var sumarea = ee.Number(area_imag.reduceRegion(
                {"reducer": ee.Reducer.sum(),
                "scale": 300,
                "geometry":boun2
                })
                .get("b1") )
  return sumarea
}
var area = GAIA.toList(10).map(getArea).reduce(ee.Reducer.sum())
print("map imgcol area:",area)//185905517767.81976
print("boun area:",boun2.area(ee.ErrorMargin(1)))//186114667454.5676


// peojection area
var wkt = ' \
  PROJCS["World_Mollweide", ["GCS_WGS_1984", ["WGS_1984",SPHEROID["WGS_1984",6378137,298.257223563]],PRIMEM["Greenwich",0],UNIT["Degree",0.017453292519943295]],PROJECTION["Mollweide"],PARAMETER["False_Easting",0],PARAMETER["False_Northing",0],PARAMETER["Central_Meridian",0],UNIT["Meter",1],AUTHORITY["EPSG","54009"]]';

var proj_mollweide = ee.Projection(wkt);
print("boun transform area:",boun2.transform(proj_mollweide,ee.ErrorMargin(1)).area(ee.ErrorMargin(1))) //186114667454.5346
print("boun areapro area:",boun2.area(ee.ErrorMargin(10),proj_mollweide))//186531461977.6914

function getAreaProject(image){
  var a = ee.Image(image).gte(0).reproject(proj_mollweide).clip(boun2)
  var area_imag = a.multiply(ee.Image.pixelArea())
  var sumarea = ee.Number(area_imag.reduceRegion(
                {"reducer": ee.Reducer.sum(),
                "scale": 30,
                "geometry":boun2
                })
                .get("b1") )
  return sumarea
}
var area = GAIA.toList(70).map(getArea).reduce(ee.Reducer.sum())
print("img reproject area:",area)//185905517767.81976

mosaic area:
185940066188.5224
map imgcol area:
185905517767.81976
boun area:
186114667454.5676
boun transform area:
186114667454.5346
boun areapro area:
186531461977.6914
img reproject area:
185905517767.81976
```

### 栅格区域太大时切片计算
var b = ee.Array(lslat).reshape([60,1])
```Javascript
var region = "SPA"
var boun = ee.FeatureCollection("users/zhouzz400/Boundries/worldRegion")
  .filter(ee.Filter.eq("Abbrv",region)).geometry()
var bound = ee.List(boun.bounds().coordinates().get(0))
var pro = boun.bounds().projection()
print(boun.bounds().coordinates())
Map.addLayer(boun.bounds())
var left = ee.Number(ee.List(bound.get(0)).get(0)).floor()
var right = ee.Number(ee.List(bound.get(1)).get(0)).ceil()
var down = ee.Number(ee.List(bound.get(0)).get(1)).floor()
var up = ee.Number(ee.List(bound.get(2)).get(1)).ceil()

//92,236,-30,29

var rec = ee.Geometry.Rectangle([left, down,right, up],null,false)

// var ls = ee.List([])
// for(var i = left; i < right; i++) {
//   for (var j = down; i< up; i++){
//     var rec = ee.Geometry.Rectangle([i, j,i.add(1), j.add(1)],null,false)
//     ls.evaluate(function(rec){  //行不通，push不进去
//       ls.add(rec)
//       return ls})
//   }
// }

// var lslat = ee.List.sequence(down,up)
// var lslng = ee.List.sequence(left,right)
var down = ee.Number(0)
var left = ee.Number(90)
var lslat = ee.List.sequence(down,down.add(10))
var lslng = ee.List.sequence(left,left.add(10))
var ls = lslat.map(function(lat){
  var y = lslng.map(function(lng){
    return ee.List([lat,lng])
  })
  return y
})

//var array = ee.Array(ls).reshape([8700,2])
var array = ee.Array(ls).reshape([121,2])
var rect = array.toList().map(function(point){
  var x = ee.Number(ee.List(point).get(0))
  var y = ee.Number(ee.List(point).get(1))
  var rec = ee.Geometry.Rectangle([y,x.subtract(1) ,y.add(1), x],null,false)
  return rec
})
//var rectg = ee.List(ee.Geometry.MultiPolygon(rect))
Map.addLayer(ee.Geometry.MultiPolygon(rect))
var area = rect.map(function(fets){
  var fet = ee.Geometry(fets)
  var GAIA = ee.ImageCollection("users/zhouzz400/GAIA")
  .filterBounds(fet).mosaic().clip(fet).gte(0)
  var a = ee.Image(GAIA)
  var area_imag = a.multiply(ee.Image.pixelArea())
  var sumarea = ee.Number(area_imag.reduceRegion(
                {"reducer": ee.Reducer.sum(),
                "scale": 300,
                "geometry":fet
                })
                .get("b1") )
  return sumarea
})
var area = area.reduce(ee.Reducer.sum())
print(area)
//3151140115.2027273
//6305539433.349972
//1482707901266.5425
```

### 造掩膜填空
```Javascript
var region = "SPA"
var boun = ee.FeatureCollection("users/zhouzz400/Boundries/worldRegion")
  .filter(ee.Filter.eq("Abbrv",region)).geometry()

var GAIA = ee.ImageCollection("users/zhouzz400/GAIA")
  .filterBounds(boun).mosaic().clip(boun)
var GAIA_viz = {min:0,max:34,palette:["000000","ff0000"]}
//Map.addLayer(GAIA,GAIA_viz)

var GAIA_masked = GAIA.updateMask(GAIA.gte(1))
var emp = ee.Image.constant(0).select(["constant"],["b1"])
//print(emp.get("system:band_names"))
//print(emp.propertyNames())
print(emp)
var x = ee.ImageCollection([GAIA_masked,emp]).mosaic()
Map.addLayer(x,{min:0,max:1,palette:["000000","ff0000"]})
print(x)
```

```Javascript
var region = "SPA"
var boun = ee.FeatureCollection("users/zhouzz400/Boundries/worldRegion")
  .filter(ee.Filter.eq("Abbrv",region)).geometry()

var GAIA = ee.ImageCollection("users/zhouzz400/GAIA")
  .filterBounds(boun).mosaic()

var emp = ee.Image(1).select(["constant"],["b1"])

var GAIA_mask = GAIA.mask().toUint8()
var mask= ee.ImageCollection([GAIA_mask,GAIA]).mosaic().reduce(ee.Reducer.min())
var g = GAIA.updateMask(mask)
Map.addLayer(g,{min:0,max:34,palette:["000000","ff0000"]})



var bound = ee.List(boun.bounds().coordinates().get(0))
var pro = boun.bounds().projection()
Map.addLayer(boun.bounds())
var left = ee.Number(ee.List(bound.get(0)).get(0)).floor()
var right = ee.Number(ee.List(bound.get(1)).get(0)).ceil()
var down = ee.Number(ee.List(bound.get(0)).get(1)).floor()
var up = ee.Number(ee.List(bound.get(2)).get(1)).ceil()

//92,236,-30,29

var rec = ee.Geometry.Rectangle([left, down,right, up],null,false)

var lslat = ee.List.sequence(down,up)
var lslng = ee.List.sequence(left,right)
var ls = lslat.map(function(lat){
  var y = lslng.map(function(lng){
    return ee.List([lat,lng])
  })
  return y
})

var array = ee.Array(ls).reshape([8700,2])
var rect = array.toList().map(function(point){
  var x = ee.Number(ee.List(point).get(0))
  var y = ee.Number(ee.List(point).get(1))
  var rec = ee.Geometry.Rectangle([y,x.subtract(1) ,y.add(1), x],null,false)
  return rec
})
//var rectg = ee.List(ee.Geometry.MultiPolygon(rect))
Map.addLayer(ee.Geometry.MultiPolygon(rect))
var area = rect.map(function(fets){
  var fet = ee.Geometry(fets)
  var a = ee.Image(g.clip(boun).gte(3))
  var area_imag = a.multiply(ee.Image.pixelArea())
  var sumarea = ee.Number(area_imag.reduceRegion(
                {"reducer": ee.Reducer.sum(),
                "scale": 300,
                "geometry":fet
                })
                .get("b1") )
  return sumarea
})
var area = area.reduce(ee.Reducer.sum())
print(area)
```
