### Simplified Steps 

1. Bring in GeoJson without JQuery, in order to create a named `var` to hold each of the layers
2. Use the `layer.bringToFront()` method to order the layers 

### Detailed steps

**1.1. First, recreate your .geojson file as a .js file containing a var to hold the geojson data**

*Note: this method for adding geojson data to a Leaflet map is (rather poorly) explained [here](https://leafletjs.com/examples/geojson/), and used in [this tutorial](https://leafletjs.com/examples/choropleth/) from Fall Quarter Lab 3b. This method only works if you are using geojson data that you can edit, **not** live data from a data feed, like the USGS earthquake data we used in Lab 2 in Fall Quarter.* 

1. Open your `Sea_Parcel.geojson` file

2. Add the following to the very beginning of the file: 

```javascript
var parcels =
```

3. Save AS `ParcelData.js`, making sure you save it in a location you can find again (such as the same folder where you saved your .geojson files)

   *Some notes about the steps above*

   * You could give the var any name you wanted, but you will reference the name you use later on in the double.html file.
   * Similarly, you could save the file with any name you want; just be sure it ends in .js and not .geojson!

4. Test your understanding: Repeat steps 1-3 to recreate the MHA file as a geojson. Name the var that will hold the geojson data `zones` and name the file `ZoningData.js`

---

**1.2. Second, add a link to the newly created JS files in the body of your main HTML file (double.html in your case) **

1. Under the line of code where you create your map div, add the following: 

   ``` html
   <script type="text/javascript" src="ParcelData.js"></script>
   <script type="text/javascript" src="ZoningData.js"></script>
   ```

   *Notes:* 

   * If you gave the JS files you created above different names than ParcelData.js or ZoningData.js, make sure to use the names you chose! 
   * If you saved your JS files inside an enclosing folder, be sure to include the folders in the pathname specified in the `src` attribute. For instance, if you put them inside a folder called 'data' that is stored at the same level as your `double.html` file, the relative pathname you would use for the parcel data file would be: `"data/ParcelData.js"`

---

**1.3. Third, alter the section of code where you create the two layers (parcel data and zoning data layers)**

1. Replace this section of code: 

   ```Javascript
   $.getJSON("MHA2.geojson",function (CLASS_DESC){
   			L.geoJson(CLASS_DESC,{
   				style: function(feature){
   			return{ color: "blue", weight: .5, fillColor: "blue", fillOpacity: .5}
   			}
   		}).addTo(mymap);
   	});
   ```

   with the following: 

   ```javascript
   var zonesLayer = L.geoJson(zones,{
   	    style: function(feature){
   			return{ color: "blue", weight: .5, fillColor: "blue", fillOpacity: .5}
   			}
   }).addTo(mymap);
   ```

   *Note, there are essentially just two things different between these code fragments*:

   * First, we've replaced `$.getJSON("MHA2.geojson",function (CLASS_DESC){` with `var zonesLayer =`. We will refer to this layer by the var name 'zonesLayer' in the next step. You can choose a different name for the var if you wish, but need to be consistent when you reference it again later. 
   * Second, we've replaced `L.geoJson(CLASS_DESC,{` with `L.geoJson(zones,{`. 'zones' is a reference to the variable name you chose when you created the `ZoningData.js` file, for the var that hold the geojson data. 
   * Everything else is the same: the style options that determine how the layer looks, as well as the use of the `addTo(mymap)` method to add the layer to the map so that it displays. 

2. Test your understanding: repeat this step for parcel data layer, naming the var that will hold the layer `var = parcelsLayer` and replacing `STATUS` with `parcels`, the var name used in the ParcelData.js file. 

---

**2.1**. Use the `bringToFront()` method to specify which layer you want to display on top. 

Once we have the layers defined as variables that we can reference later in the code, it's quite easy to order them. We talked earlier about doing so with a [layer control](https://leafletjs.com/examples/layers-control/), which includes an [auto Z-indexing option](https://leafletjs.com/reference.html#control-layers-autozindex) by default. However, I discovered an even easier way that uses just one line of code. 

After the section of code where you create the two layers (i.e. what you did in step 1), add the following: 

```javascript
parcelsLayer.bringToFront();
```

Here we are simply applying the `bringToFront()` method to the layer named `parcelsLayer`. Boom: the parcels will consistently load on top of the zoning layer. Documentation for the bringToFront() method is [here](https://leafletjs.com/reference.html#path-bringtofront). Note: if you had more than two layers that you wanted to arrange in a specific order, the layer control is a better way to go, but this works well when we are only ordering two layers. 