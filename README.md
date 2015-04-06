# Maptime Boulder: Pub Map

![EndResult](https://github.com/PetersonGIS/Maptime-Boulder-Pub-Map/blob/master/images/FinalMapSmall.jpg)

This is a tutorial for making a historic pub map with QGIS using data from Geofabrik (OpenStreetMap), UK Data Service, Ordnance Survey, and a historic pubs point dataset created for this tutorial. This tutorial is also available as a [live demo presentation, presented at FOSS4GNA 2015](https://www.youtube.com/watch?v=lCFbMPP_1xQ). 

## Download and install QGIS

This tutorial requires QGIS 2.8.x. Download and install from [QGIS.org](http://www.qgis.org/en/site/forusers/download.html). If you have a previous version of QGIS already installed, it is ok to have both versions installed as they are stored in folders according to their release number.

## Add the tutorial data to QGIS

Open QGIS Desktop. Download the three datasets from the [data folder](https://github.com/PetersonGIS/Maptime-Boulder-Pub-Map/tree/master/data) locally and add them to your qgis project by dragging the .shp files to the layer list. These files are buildings, dlua_regions, and wards. Also download the historic pubs data from its [repository](https://github.com/PetersonGIS/GeoJSON-HistoricPubs) and add it (.geojson file) to the QGIS project as well by dragging it to the layer list.

![dragdropdemo](https://github.com/PetersonGIS/Maptime-Boulder-Pub-Map/blob/master/images/dragdropdata.jpg)

###Data sources
+ buildings: [Geofabrik](http://download.geofabrik.de/europe/great-britain/england.html), Greater London, .shp.zip
+ dlua_regions: [Ordnance Survey](http://www.ordnancesurvey.co.uk/business-and-government/products/meridian2.html), Meridian 2
+ wards: [UK Data Service](http://census.ukdataservice.ac.uk/get-data/boundary-data.aspx), Easy Download
+ HistoricPubs: [GitHub Repository](https://github.com/PetersonGIS/GeoJSON-HistoricPubs), Download Zip

###Take it to the next level
+ Explore Geofabrik, Ordnance Survey, and UK Data Service download sites on your own and find other data to put on the map.
+ Modify or add to the historic pubs repository

## Change the projection if needed
+ Check the bottom-right corner of the QGIS screen. If it says EPSG: 4326, we need to change the projection. If it says EPSG: 27700, it won't need to be changed as that's the British National Grid, which is what we want. There are several ways to change the projection. Here are two:
- Simply re-start QGIS and, when you add the four datasets to the project, be sure to drag either the wards or the dlua_region shapefiles first. QGIS uses the first projection added to the project as the default projection and since these two shapefiles are already in 27700, they will force the project default to that projection.
- Click Project>Project Properties>CRS>enable on the fly CRC transformation>Filter for "27700">click the British National Grid entry in the result window>OK

## Select polygons based on an expression
+ Right-click the wards dataset in the Layers List
+ Open Attribute Table
The OLDCODE entries that start with 00AA are the ones we want to show on this map. We don't want to show any of the other wards. So let's select only the wards we want and make a new dataset.
+ Click the Select by expression button in the table. It's the epsilon symbol with the yellow square under it.
+ Expand the Fields and Values by clicking the plus sign
+ Double-click OLDCODE, this adds it to the expression box encased in quotes
+ Finish the expression by typing in the expression box so that the expression looks like this: "OLDCODE" LIKE '00AA%'. Once you do this the Output preview at the bottom may say "1"
+ Click Select in the lower-right corner, the map should show the wards inside the London Square Mile as being highlighted in yellow

## Merge selected features
+ Click the wards dataset in the Layers List so it's active. Most tools operate like this in QGIS: you tell it which dataset you’re wanting to use the tool with by selecting it in the Layer List
+ Click Toggle Editing: it's the single pencil icon
+ Go to Edit > merge selected features
+ If you cared about the attributes and how they are merged you would use this dialog to specify. In this case we care only about the geometry so just click OK.
+ Click toggle editing again and save the edits. Now the polygons are merged into one polygon that represents the London Square Mile, which is approximately the outline of the original walled city of London. Notice how all the small wards have been merged so they are a single feature now instead of many small ones.

## Select a feature using the select tool
+ Click the Select Features button. It's the one with the arrow pointing toward the center of a yellow square.
+ Click the square mile shape so that it is selected. You can tell it is selected because it will turn yellow.

## Create a new dataset out of a selected set of features
+ Right-click the wards dataset in the Layers List
+ Save As
+ Format: Esri Shapefile
+ Save as SquareMile in your project folder
+ Make sure to check the box next to Save only selected features and Add saved file to map, leave everything else
+ OK
+ cleanup the Layers List by deleting the wards dataset, we don't need it any more

## Change the layer list order
In the QGIS Layer List, click and drag the four files so that they are in this order:
+ HistoricPubs
+ SquareMile
+ buildings
+ dlua_regions



