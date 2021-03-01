# Class 5: CARTO tutorial

Computing for SUE class @nyu, Spring 2021.
avigailvantu@nyu.edu

### 1. Create an account on CARTO:
NYU provides free access to CARTO. Everyone part of the NYU community can create an account through [this URL](https://nyu.carto.com/maps) Use your NYU email and PW to log-in. For more information about eligibility and other CARTO tutorials go [here](https://www.nyu.edu/life/information-technology/getting-started/software/carto.html)

### 2. Data: Housing Maintenance Code Violations
Today we will be working with the housing violations data, which comes from the Housing Preservation Department. This is a comprehensive data of all housing violations recorded in NYC. Violations are issued either by complaint of a tenant or by an inspector conducting regular inspections in NYC’s buildings. Housing violations are labeled by the inspector with one of these classes:
- Class A: are Non-Hazardous violations that need to be corrected within 90 days of filing the violation
- Class B: are Hazardous violations that need to be corrected within 30 days (with some exception for immediate correction required)
- Class C: are Immediately Hazardous violations. These violations are required to be taken care of by landlords within 24 hours. C class violation are usually related to hot water/heat/lead paint/window guards.

#### Access the data on the Open Data platform.

For this exercise we only want to work with Class C violations. To do so, we’ll go to the [open data website](https://data.cityofnewyork.us/Housing-Development/Housing-Maintenance-Code-Violations/wvxf-dwi5/data)
We’ll then apply these filters:

1. Class is : “C”
2. Brough is : “BROOKLYN”
3. Inspection date is between: 01/01/2020 and 12/31/2020
4. This should result in ~53K results

5.  Export as “CSV”

### 3. Load the violations data on CARTO:    
- Go to “Dataset” —> "connect dataset" and upload the file from your computer and then finalize the upload clicking “Upload dataset”
- Give it a few minutes to “connect” as CARTO loads your data.
- Finally, when CARTO is done loading the date it will display it as a table
- CARTO should be able to identify the geometric column in the data that it will use to visualize.
- Also note that CARTO displays the columns in A-Z order.

![alt text](https://github.com/avigailvantu/c4sue/blob/master/tutorials/Class_5/CARTO/carto_data.png)

- Click on “Create Map” on the bottom right to view the data on a map
- Once you do so all points should appear on your map
- Remember, we are looking into all *sever* violations in *Brooklyn*. So our map view would reflect only these type of violations and not all of the violations.
- Similar to QGIS CARTO can accommodate multiple datasets and layers. In the main interface you should be able to view all layers. At the moment we only have one that would appear as the CSV name.
- To edit and symbolize click on the layer

### 4. The components of CARTO’s interface

- You should now see five tabs on the side menu: 1. Data, 2. Analysis, 3. Style, 4. Pop-up and  5. Legend

Let’s go through them one by one before symbolizing this layer
  *  “Data” displays basic information about the data and each column. Among other information, CARTO will tell you the number of NULL values.
  * “Analysis” is a service that will do the geocoding and other data wrangling and manipulations for you. Many of the tools in this tab are similar to QGIS’ tools.
  * The “Style” tab will allow us to visualize and symbolize the data (similar to the symbology tab in the QGIS properties). Among other uses, you can control the size and color of the points or polygon layer.
  * “Pop-up” allows you to decide which features will be visible when hovering over them on a webpage
  * Finally “Legend” will display the legend, again similar to QGIS  

Here’s how my initial map looks like:

![alt text](https://github.com/avigailvantu/c4sue/blob/master/tutorials/Class_5/CARTO/initialmap.png)

#### Aggregation style 01:
  * Because there are so many points all around the borough, it is hard to draw conclusions on trends solely by looking at the data as is.
  * To be able to view how the hazardous (C) housing violations are distributed in the city we can try different methods and display of the data.
  * First, aggregate the data by squares/hexbins. Doing so we see that east-north Brooklyn has much less violations than south and west Brooklyn. That is surely an interesting fact. But if we think about it totally makes sense and aligns with what we learned about standardization of the the data. These are all less populations dense areas.

![alt text](https://github.com/avigailvantu/c4sue/blob/master/tutorials/Class_5/CARTO/squares_agg.png)

#### Aggregation style 02:
  * Another way to view how the data distributes is by displaying a heat-map. Heat-maps are a technique to color code the data in order to represent different values in the data. Usually it would display clusters and less concentrated data.
  * CARTO’s default displays the points size as 45 which is way too big for us, since we have so many violations! By moving the curser to the left, reduce the point size to 6 - 10. This size will enable you to see the concentration of points in space while still identifying clusters. Also, try changing the point color and resolution.

### 5. Use CARTOCSS:
- CARTO allows manipulation of the data using programming languages, SQL used to be the default, but CSS is now used to filter and visualize.
- Let’s say we only want to make more specific changes like marker size changing according to zoom level.
- There are a lot of examples provided by CARTO on their website.  


#### Display size of violations using different zoom level
1. In the DATA tab: switch from "values" to "CARTOCSS" mode. This will open a CSS script page where we will be able to write our commands. The script written there represents the styling we just did.
3. Copy-paste this code into the "Layer" section of the CSS code: (replace it with the existing code)



```CSS
marker-fill-opacity: 0.9;
marker-line-color: #0f2f36;
marker-line-width: 0;
marker-opacity: 0.7;
marker-line-opacity: 0.3;
marker-placement: point;
marker-type: ellipse;
marker-width: 2;
marker-fill: #af5cda;
marker-allow-overlap: true;
[zoom = 4] {marker-width: 2}
[zoom = 5] {marker-width: 4}
[zoom > 5] {marker-width: 6}

```



6. Let's say we want to create categories based on the floor of the violation:

```CSS
marker-fill: #ff058d;
 marker-fill-opacity: 0.5;
 marker-allow-overlap: true;
 marker-line-width: 0.2;
 marker-line-color: #FFF;
 marker-line-opacity: 1;
 marker-width: 1;

 /* categories */
 [ story > 1] {
   marker-width: 2;
 }
 [ story > 2 ] {
   marker-width: 4;
 }
 [ story > 3 ] {
   marker-width: 12.5;
 }
 [ story > 4 ] {
   marker-width: 20.5;
 }
 ```

Take your time to explore this more.

### 7.Publish your map:
  1. CARTO is different than QGIS since it is an *interactive* map tool that allows us to publish maps, on top of downloading them as an image. This means, we can easily embed a map on a web interface, or just share a URL of it to stack-holders.
  2. To publish this map we need to go to the layers view
  3. Then click on “Publish” on the bottom right side of the map.

![alt text](https://github.com/avigailvantu/c4sue/blob/master/tutorials/Class_5/CARTO/publish.png)

  4. After clicking on publish, you will be directed to a webpage where you will need to choose between "share with colleagues" and "publish".
  5. Before finalizing this, you will need to make the map public (NYU's default maps are private).
  6. Go back to the layers page and click on the orange private on the top left
  7. when the dropdown opens, change from Private to Public

  ![alt text](https://github.com/avigailvantu/c4sue/blob/master/tutorials/Class_5/CARTO/private_public.png)

  6. Finally we can publish!
  7. Click on publish again. This time when you go to the Publish tab you can access two options for publishing your map: embed or get a link.
  8. copy the address in the "Get a link" section to your browser.


### 8. Deliverable:
Please submit your CARTO map *URL* (make sure it's public!) on NYU Classes.
