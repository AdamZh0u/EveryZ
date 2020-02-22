# Course1
## String

```javascript
// create
var string = ee.String("helloworld");
// display
print(string);

// change
var cat_string = string.cat("demo");
print(cat_string);
var rep_string = cat_string.replace("d","zz","g");//global match
print(rep_string);

// split
var spl_string = string.split("o");
print(spl_string);

// match
var mat_string = string.match("o");
print(mat_string);

// slice
var sli_string = string.slice(1,5);
print(sli_string);

// length
var len_string = string.length()
print(string, len_string)

// ## number
var numb1 = ee.Number(1237834050);
var numb2 = ee.Number(-3.1435963);

// transfer
var int_numb2 = numb2.int8()
  // int = toInt double = toDouble float = toFloat
print(int_numb2)

// compare
  // eq neq gt gte lt lte
  // and or not

// calculate
  //floor round ceil  abs sqrt exp log log10

// bitwise
var numb3 = ee.Number(1);
var numb4 = ee.Number(2);
var numb_And = numb3.bitwiseAnd(numb4);
var num_Or = numb3.bitwiseOr(numb4);
print(numb_And,num_Or);
  // leftshift

// a great examp
    // var meal= rice(50).wash(100, fliter).zheng(100).cheng(12,A>B)
```
## dictionary

```javascript
// create ee.Dictionary()
var Dic_1 = ee.Dictionary({
  Name:"demo",
  Age:"20"
})
var Dic_2 = ee.Dictionary({
  Weight:"30",
  Hight:"30"
})

// change dic.combine() dic.set()
var Dic_combine = Dic_1.combine(Dic_2,true);//use second first when conflict
print(Dic_combine);

var Dic_3 = Dic_1.set("Age","30"); // add or change
print(Dic_3);

// iquiry dic.keys dic.get dic.values
print(Dic_1.keys());
print(Dic_1.values().slice(1,2));
print(Dic_1.get("Name"));

// compare dic.contains
print(Dic_1.contains("Height")); // if exsist?

// size dic.size()
print(Dic_1.size());

```

 ## reducer
### create math transfer
```javascript
// .count/.countEvery/.first()
var Reducer_Count = ee.Reducer.count();
var Reducer_CountEvery = ee.Reducer.countEvery();
var Reducer_First = ee.Reducer.first();

var Provinces_Number_1 = China_Provinces.reduceColumns(
  Reducer_Count,["Name"]);
var Provinces_Number_2 = China_Provinces.reduceColumns(
  Reducer_CountEvery,[]); // count every columns
var Provinces_First = China_Provinces.reduceColumns(
  Reducer_First,["Name"]);

Map.addLayer(China_Provinces);
print(China_Provinces);
print("Reducer_Count",Provinces_Number_1);
print("Reducer_CountEvery",Provinces_Number_2);
print("Refucer_First",Provinces_First);

// .frequencyHistogram()
print(China_Cities.limit(10));
var FrequencyHiso_Reducer = ee.Reducer.frequencyHistogram();
var City_Frequency = China_Cities.reduceColumns(FrequencyHiso_Reducer,["省份"]);

var Fig_Histo = ui.Chart.feature.histogram(China_Cities,"省份");
print(City_Frequency,Fig_Histo);
Map.addLayer(China_Cities);

// .allNonZero/.anyNonZero()
var No_Zero_Reducer = ee.Reducer.allNonZero();
var Any_Non_Zero_Reducer = ee.Reducer.anyNonZero();
var List_Test_1 = ee.List([1,2,3,5,9]);
var List_Test_2 = ee.List([1,4,5,6,0]);

var Result_1 = List_Test_1.reduce(No_Zero_Reducer);
var Result_2 = List_Test_1.reduce(Any_Non_Zero_Reducer);
var Result_3 = List_Test_2.reduce(No_Zero_Reducer);
var Result_4 = List_Test_2.reduce(Any_Non_Zero_Reducer);

print("Result_1",Result_1);
print("Result_2",Result_2);
print("Result_3",Result_3);
print("Result_4",Result_4);

// .toList()
print(China_Cities.first());
var Tolist_Reducer = ee.Reducer.toList();
var City_List = China_Cities.reduceColumns(Tolist_Reducer, ["Prefecture"]);
print(City_List);

// .toCollection()
var Reducer_to_Collection = ee.Reducer.toCollection(["provinces","cities"]);//rename
print(Reducer_to_Collection);
var City_Collection = China_Cities.reduceColumns(Reducer_to_Collection,["省份","Prefecture"]);
print(City_Collection);

// .product/ sum/ mean/variance/sampleVariance/stdDev/sampleStdDev
function Add_Area(feature){
  var The_Area = ee.Number(feature.area());
  return feature.set("Area_km2", The_Area.divide(1000*1000));
}
var City_WithArea = China_Cities.map(Add_Area);
print(City_WithArea)
var Reducer_Product = ee.Reducer.product();
  //var Reducer_Product = ee.Reducer.product();sum,mean,variance,sampleVariance,stdDev
var Area_Product = City_WithArea.reduceColumns(Reducer_Product,["Area_km2"]);
print("Area_Product", Area_Product)

// .max/min/minMax/median/mode
var Reducer_Max = ee.Reducer.max()
var Area_Max = City_WithArea.reduceColumns(Reducer_Max,["Area_km2"])
print("Area_Max",Area_Max)

// image max
var image = image.select(["B4","B3","B2"]);
var maxValue = image.reduce(ee.Reducer.max());
Map.centerObject(image,8);//zoom
Map.addLayer(maxValue,{max:13000},"Maximum value image");

// intervalMean/percentile
  // 0, 50 mean

// linearFit()
var Data_X = ee.List([12,13,14,5]);
var Data_Y = ee.List([14,12,41,14]);

var Linear_Reducer = ee.Reducer.linearFit();
var Fited = ee.List([Data_X,Data_Y]).reduce(Linear_Reducer);
print(Fited);

// linearFit use to pridict weather
var createTimeBand = function(image){
  return image.addBands(image.metadata("system:time_start").divide(1e18));
}
var collection = ee.ImageCollection("NASA/NEX-DCP30_ENSEMBLE_STATS")
  .filter(ee.Filter.eq("scenario","rcp85"))
  .filterDate(ee.Date("2006-01-01"),ee.Date("2050-01-01"))
  .map(createTimeBand);
var linearFit = collection.select(["system:time_start","pr_mean"])
  .reduce(ee.Reducer.linearFit());
print(linearFit);
Map.addLayer(linearFit,
  {min:0,
   max:[-0.9,8e-5,1],
   bands:["scale","offset","scale"]},
  "fit");
// setOutputs/getOutputs
var Reducer_Original = ee.Reducer.minMax();
var Reducer_Modified = Reducer_Original.setOutputs(["Range_Low","Range_High"]);
print("Original",Reducer_Original.getOutputs());
print("Modified",Reducer_Modified.getOutputs());

// combine
var Reducer_Max = ee.Reducer.max();
var Reducer_Min = ee.Reducer.min();
var Reducer_Combine = Reducer_Max.combine(Reducer_Min);

var Array_Example = ee.Array([[1,2],
                              [3,4]]); // axis = 0 updown

var Combine_Reduced_1 = Array_Example.reduce(
  Reducer_Combine, [0], 1);// direction 0 field axis
var Combine_Reduced_2 = Array_Example.reduce(
  Reducer_Combine, [1], 0);

print("Max of [1,3] and min of [2,4]",Combine_Reduced_1);
print("Max of [1,2] and min of [3,4]",Combine_Reduced_2);

// repeat
var China_Cities = ee.FeatureCollection("users/zhouzz400/Boundries/China_Cities");
var Reducer_Repeat = ee.Reducer.frequencyHistogram().repeat(2);
var Province_City_Frequency = China_Cities.reduceColumns(Reducer_Repeat,["Prefecture","省份"]);
print(Province_City_Frequency);

// group
var countries = ee.FeatureCollection("ft:1S4EB6319wWW2sWQDPhDvmSBIVrD3iEmCLYB7nMM");
var sums = countries
  .filter(
    ee.Filter.and(
      ee.Filter.neq("Census 2000 Population",null),
      ee.Filter.neq("Census 2000 Housing Units", null))
  )
  .reduceColumns({
    selectors:["Census 2000 Population",
      "Census 2000 Housing Units","StateName"],
    reducer:ee.Reducer.sum().repeat(2).group({
      groupField:2,
      groupName:"state",})
  });
print(sums);

```

## kernel
```javascript
// DEM_Roberts
var Provinces = ee.FeatureCollection("users/zhouzz400/Boundries/China_Provinces")
var CQ_table = Provinces.reduceColumns(ee.Reducer.frequencyHistogram(),["Name"])
var CQ = Provinces.filterMetadata("Name","equals","上海市").geometry()

var DEM = ee.Image("CGIAR/SRTM90_V4").clip(CQ);

var DEM_Roberts = DEM.convolve(ee.Kernel.roberts());//卷积
var DEM_prewitt = DEM.convolve(ee.Kernel.prewitt());
var DEM_sobel = DEM.convolve(ee.Kernel.sobel());
var DEM_compass = DEM.convolve(ee.Kernel.compass());
var DEM_kirsch = DEM.convolve(ee.Kernel.kirsch());

Map.addLayer(DEM,{min:0,max:2000},"DEM");
Map.centerObject(CQ,7)
Map.addLayer(DEM_Roberts,{min:-60,max:60},"DEM_Roberts")
Map.addLayer(DEM_prewitt,{min:-270,max:270},"DEM_prewitt")
Map.addLayer(DEM_sobel,{min:-370,max:370},"DEM_sobel")
Map.addLayer(DEM_compass,{min:-300,max:300},"DEM_compass")
Map.addLayer(DEM_kirsch,{min:-1100,max:1100},"DEM_kirsch")

// laplacian4 laplacian8

// based on distance
  // euclidean/gaussian/manhattan/chebyshev

// shape kernel
  // circle octagon square diamond cross plus fied

// operation
  // rotate 90*   add

// print kernel
print(ee.Kernel.euclidean(1))
print(ee.Kernel.gaussian(1))

// function name(parameters){operation}

```
