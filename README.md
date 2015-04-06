# Maptime Boulder: Pub Map

![EndResult](https://github.com/PetersonGIS/Maptime-Boulder-Pub-Map/blob/master/images/FinalMapSmall.jpg)

This is a tutorial for making a historic pub map with QGIS using data from Geofabrik (OpenStreetMap), UK Data Service, Ordnance Survey, and a historic pubs point dataset created for this tutorial. This tutorial is also available as a [live demo presentation, presented at FOSS4GNA 2015](https://www.youtube.com/watch?v=lCFbMPP_1xQ). 

### Download and install QGIS

This tutorial requires QGIS 2.8.x. Download and install from [QGIS.org](http://www.qgis.org/en/site/forusers/download.html). If you have a previous version of QGIS already installed, it is ok to have both versions installed as they are stored in folders according to their release number.

### Add the tutorial data to QGIS

Open QGIS Desktop. Download the three datasets from the [data folder](https://github.com/PetersonGIS/Maptime-Boulder-Pub-Map/tree/master/data) locally and add them to your qgis project by dragging the .shp files to the layer list. These files are buildings, dlua_regions, and wards. Also download the historic pubs data from its [repository](https://github.com/PetersonGIS/GeoJSON-HistoricPubs) and add it (.geojson file) to the QGIS project as well by dragging it to the layer list.

![dragdropdemo](https://github.com/PetersonGIS/Maptime-Boulder-Pub-Map/blob/master/images/dragdropdata.jpg)

####Data sources
+ buildings: [Geofabrik](http://download.geofabrik.de/europe/great-britain/england.html), Greater London, .shp.zip
+ dlua_regions: [Ordnance Survey](http://www.ordnancesurvey.co.uk/business-and-government/products/meridian2.html), Meridian 2
+ wards: [UK Data Service](http://census.ukdataservice.ac.uk/get-data/boundary-data.aspx), Easy Download
+ HistoricPubs: [GitHub Repository](https://github.com/PetersonGIS/GeoJSON-HistoricPubs), Download Zip

####Take it to the next level
+ Explore Geofabrik, Ordnance Survey, and UK Data Service download sites on your own and find other data to put on the map.
+ Modify or add to the historic pubs repository

### Change the projection if needed
+ Check the bottom-right corner of the QGIS screen. If it says EPSG: 4326, we need to change the projection. If it says EPSG: 27700, it won't need to be changed as that's the British National Grid, which is what we want. There are several ways to change the projection. Here are two:
++ Simply re-start QGIS and, when you add the four datasets to the project, be sure to drag either the wards or the dlua_region shapefiles first. QGIS uses the first projection added to the project as the default projection and since these two shapefiles are already in 27700, they will force the project default to that projection.
++ Click Project>Project Properties>CRS>enable on the fly CRC transformation>Filter for "27700">click the British National Grid entry in the result window>OK

### Select polygons based on an expression
+ Right-click the wards dataset in the Layers List
+ Open Attribute Table
The OLDCODE entries that start with 00AA are the ones we want to show on this map. We don't want to show any of the other wards. So let's select only the wards we want and make a new dataset.
+ Click the Select by expression button in the table. It's the epsilon symbol with the yellow square under it.
+ Expand the Fields and Values by clicking the plus sign
+ Double-click OLDCODE, this adds it to the expression box encased in quotes
+ Finish the expression by typing in the expression box so that the expression looks like this: "OLDCODE" LIKE '00AA%'. Once you do this the Output preview at the bottom may say "1"
+ Click Select in the lower-right corner, the map should show the wards inside the London Square Mile as being highlighted in yellow

### Merge selected features
+ Click the wards dataset in the Layers List so it's active. Most tools operate like this in QGIS: you tell it which dataset you’re wanting to use the tool with by selecting it in the Layer List
+ Click Toggle Editing: it's the single pencil icon
+ Go to Edit > merge selected features
+ If you cared about the attributes and how they are merged you would use this dialog to specify. In this case we care only about the geometry so just click OK.
+ Click toggle editing again and save the edits. Now the polygons are merged into one polygon that represents the London Square Mile, which is approximately the outline of the original walled city of London. Notice how all the small wards have been merged so they are a single feature now instead of many small ones.

### Select a feature using the select tool
+ Click the Select Features button. It's the one with the arrow pointing toward the center of a yellow square.
+ Click the square mile shape so that it is selected. You can tell it is selected because it will turn yellow.

### Create a new dataset out of a selected set of features
+ Right-click the wards dataset in the Layers List
+ Save As
+ Format: Esri Shapefile
+ Save as SquareMile in your project folder
+ Make sure to check the box next to Save only selected features and Add saved file to map, leave everything else
+ OK
+ cleanup the Layers List by deleting the wards dataset, we don't need it any more

### Change the layer list order
In the QGIS Layer List, click and drag the four files so that they are in this order:
+ HistoricPubs
+ SquareMile
+ buildings
+ dlua_regions

### Assign colors
+ Double-click dlua_region>Style>Color>Choose Color
+ In the boxes next to R, G, and B put in these numbers: 41, 1, 42
+ Color the buildings dataset the same way as dlua_region, except with this RGB triplet: 131, 99, 236
+ Color the background by clicking Project>project properties>background color, and use the RGB triplet: 87, 48, 112
+ hold off on coloring the SquareMile for now
#### Take it to the next level
Choose your own color palette. Try [coolors](app.coolors.co). Hat tip Rachel Stevenson.

### Icons for the pub points
+ If you're running out of time symbolize the pubs as white dots, otherwise:
+ Go to Layer Properties for the HistoricPubs dataset by double clicking the HistoricPubs dataset in the Layers List
+ Click Symbol layer type: SVG marker
+ click the ellipses next to the path box underneath the icons and choose the path to the dart.svg.
+ Change the size to 14
+ Try to change the color to white but notice how the dart's color doesn't actually change to white, this is because the color is not a parameter in this svg file. To change it's color dynamically in QGIS we have to parameterize the color code in the svg file. Make a copy of dart.svg in your project folder and call it dart-copy.svg. Open this in a text editor. Replace where it says <path fill="#000000" with the code from codeforsvgparam.txt. Save.
+ Re-do the above steps except this time load up dart-copy.svg. Remember to change the size to 14.
+ Change it's color to white or bright green (or whatever works with your color scheme if you chose something custom)
+ Experiment with the angle. Try changing the angle to 270 so they point at the actual point.
+ Change the anchor point from hcenter to lcenter
+ Press apply to look at the changes as you work and click OK when satisfied.

### Add labels
We'll be doing expression based labeling here. If you need to save time, simply label the HistoricPubs with their name instead of an expression.
+ Go to the Layer Properties for the HistoricPubs dataset again but this time go the labels section
+ Activate the check box next to Label this layer with and use the epsilon expression button to bring up the expression builder
+ Enter in the following expression: name || ' ' || Establishd
#### Take it to the next level
If you want to, use this time to play around with the expression builder and build your own expression
+ Change the font to something you like
#### Take it to the next level
Download the free font called []yanone kaffeesatz](https://www.yanone.de/typedesign/kaffeesatz/) and install it on your machine and use it for all the text in this map.
+ Change the text color to white if using the tutorial's color palette
+ Change the text size to 8 (the size you chose will be somewhat dependent on the typeface you are using. 8 goes well with yanone kaffeesatz bold type)
+ Press apply and notice that the labels appear but are all placed to the right of the darts. Let's move them under the darts.
+ Go back to the Layer Properties label section and choose placement>offset from point>click the box underneath, center, on the quad diagram
+ Press apply. See how the text is placed under the darts but you might want to wrap the text
+ In the Layer Properties label section go to Formatting>wrap on character>put in a space and also have it center aligned, press apply.
#### Take it to the next level
Create a slight buffer around the text to make it really show up against the dark background. Go to the label>buffer section and click a color that is the same as the background color on the map. Try using the dropper tool to get that color instead of using the RGB method.

### Inverted shapeburst fill
+ Activate SquareMile in the Layers List by clicking it
+ Right-click SquareMile > zoom to layer
+ Go to the layer properties dialog for the SquareMile > style section
+ Where it says "Single Symbol" at the top, click the drop-down arrow and choose Inverted polygons
+ Make sure the Simple fill box is highlighted (not Fill) and then change the Symbol layer type to Shapeburst fill
+ For the Gradient colors, choose Two color and change the first color to white and the second to transparent
+ Press ok

### Make a map layout with the print composer
A lot of the layout options are available on the right-hand side of the print composer. When you click on something in the layout the choices on the right side will change. That's where you can change text angle, font, and so on for text elements. It's where you can change the background color of the page (try changing it to the same color as the darkest color on the map itself, maybe with the color picker tool). It's also where you update the map if you've changed anything over in the main map project. Experiment with your layout to get it looking similar to the example or to whatever looks good to you. These instructions in this section are a little more vague to give you some freedom here and a chance to learn from experimentation.
+ Project>New print composer>click ok for a default name
+ Change the paper and quality to ansi a for a regular US letter size
+ Click the add new map button on the left and click and drag a rectangle where you want the map to go. If you have some extra time, before doing this you could set up some guides so that the rectangle is even on all sides. Make the map cover most of the page, leaving a small border.
+ Use the add text button to add a title at the top at an angle and at the bottom.







