# Trabajo-pr-ctico-3
Cuenca tempisque

//NDVI
var image = ee.Image(L8
.filterBounds(roi)
.filterDate('2019-01-01','2019-03-01')
.sort('CLOUD_COVER')
.first());

var trueColor = {bands: ['B2','B3','B2'],min:0, max: 0.3};
Map.addLayer(image,trueColor, 'image');
Map.addLayer(roi)

var ndvi = image.normalizedDifference(['B5','B4'])
var vegPalette = ['white','green'];

Map.addLayer (ndvi, {min: -1, max: 1, palette: vegPalette}, 'NDVI')

//EVI
var evi =image.expression(
  '2.5*((NIR - RED)/(NIR + 6 * RED -7.5 * BLUE +1))',{
    'NIR': image.select ('B5'),
    'RED': image.select('B4'),
    'BLUE': image.select('B2')
  });
  Map.addLayer(evi, {min:-1,max:1,palette:['FF0000', '00FF00']}, 'EVI'); 

//NDWI
var ndwi = image.normalizedDifference(['B5', 'B6']);
var waterPalette = ['white', 'blue'];
Map.addLayer(ndwi, {min: -0.5, max: 1, palette: waterPalette}, 'NDWI');

//NDWBI

var ndwi = image.normalizedDifference(['B3', 'B5']);
Map.addLayer(ndwi, {min: -1, max: 0.5, palette: waterPalette}, 'NDWBI');

//NDBI 

var ndbi = image.normalizedDifference(['B6', 'B5']);
var barePalette = waterPalette.slice().reverse();
Map.addLayer(ndbi, {min: -1, max: 0.5, palette: barePalette}, 'NDBI');

//BAI

var burnImage = ee.Image(L8
.filterBounds(roi)
.filterDate('2020-01-27', '2020-03-05')
.sort('CLOUD_COVER')
.first());
Map.addLayer(burnImage, trueColor, 'burn image');

var bai = burnImage.expression(
'1.0 / ((0.1 - RED)**2 + (0.06 - NIR)**2)', {
'NIR': burnImage.select('B5'),
'RED': burnImage.select('B4'),
});

var burnPalette = ['green', 'blue', 'yellow', 'red'];
Map.addLayer(bai, {min: 0, max: 400, palette: burnPalette}, 'BAI');
