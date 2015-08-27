# Add Valhalla routing to a web map

Valhalla is a free and open-source service that adds routing and navigation to web or mobile applications. In this walkthrough, you will learn how to make a web map featuring the Valhalla routing engine. The map you create will display the route line between points, a text narrative of maneuvers, a length and time en route summary of travel between points, functionality to drag the route start and endpoints to get a different path, and the ability change the mode of transportation, such as automobile or pedestrian.

The map also uses other Mapzen technology, including the [vector tile service](https://mapzen.com/projects/vector-tiles/) and the [Tangram renderer](https://mapzen.com/projects/tangram) to draw the features on the map.

To complete the tutorial, you should have some familiarity with HTML and JavaScript, although all the code is provided. You will need a [free API key](https://mapzen.com/developers) to use Valhalla, which requires a GitHub account for authorization. You can use any text editor and operating system, but must maintain an Internet connection to complete the tutorial. The tutorial should take about an hour to complete.

In this walkthrough, you will be taking your family on a [vacation](https://en.wikipedia.org/wiki/National_Lampoon%27s_Vacation) by car from your home of Chicago, Illinois to visit a theme park in Southern California. In your code, you will enter the start and end points of your trip and Valhalla will calculate the route. Valhalla also provides the ability for you to specify points through which your route should pass, such as visiting family in Kansas and Arizona and bobbing your head at Grand Canyon National Park, but that is beyond the scope of this introduction. 

## Sign up for a Valhalla API key
To use the Valhalla routing service, you must first obtain a free developer API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys.

Valhalla is a free, shared routing service. As such, there are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

1. Go to https://mapzen.com/developers.
2. Sign in with your GitHub account. If you have not done this before, you need to agree to the terms first.
3. Create a new key for Valhalla, and optionally, give it a name so you can remember the purpose of the project.
4. Keep the web page open so you can copy the key into the code later.

## Download and install the dependencies

The map uses the Leaflet framework, which is a JavaScript library to make web and mobile maps and provides tools for zooming, displaying attributions on a map, and drawing symbols. Leaflet is extensible, and developers have built many additional tools to enhance its functionality, including the Leaflet Routing Machine (LRM), which adds routing to a Leaflet map. The LRM-Valhalla plug-in further extends the routing functionality to use the Valhalla routing engine.

To set up your development environment, you need to install Leaflet, the Leaflet Routing Machine, and LRM-Valhalla. There are several ways you can obtain these files, including through a package manager, such as [npm](https://www.npmjs.com/). If you want to use other methods, see the documentation for those products.

1. Create a working folder on your machine where you can place the downloaded files.
2. Download Leaflet from http://leafletjs.com/download.html.
3. Download Leaflet Routing Machine from http://www.liedman.net/leaflet-routing-machine/download/.
4. Download LRM-Valhalla from http://mapzen.com/resources/lrm-valhalla.zip.
5. Download a [Tangram scene file](https://github.com/valhalla/lrm-valhalla/blob/master/examples/scene.yaml) or use your own and place it in the root level of your working folder.
6. Unzip the files you downloaded and move the subfolders to your main working folder.

Note that the folder paths shown in this walkthrough use simplified versions of the default folder names (for example, \leaflet instead of a folder named \leaflet-x.x.x with release numbers). You can rename your folders or substitute your paths as needed. There are [guidelines for organizing code for Leaflet plug-ins](https://github.com/Leaflet/Leaflet/blob/master/PLUGIN-GUIDE.md), so you can use a more sophisticated structure for your own work as you progress in your development.

## Create an index page

Now that you have downloaded the required dependent files, you are ready to start building your application. If you are using the [Node.js](https://nodejs.org/) framework or are adding routing to an existing web application, you may want to use a different file organization. This example uses the simplest structure, a single index.html file.

1. At the root level of your working folder, create a file called index.html and open it in a text editor.
2. Add the basic HTML tags, including `<!DOCTYPE HTML>`, `<html>`, `<head>`, and `<body>`. Your HTML might look like this:
  ```html  
  <!DOCTYPE html>
  <html>
  <head>
  </head>
  <body>
  </body>
  </html>
  ```

3. In the <head> tag, add a title, such as `<title>My Valhalla Map</title>`.
4. Save your edits to the index.html file.

## Get the project running locally

To see the changes to your map, you need to run it locally on your machine. You need to start a web server to do this because the scripts could be blocked by your browser’s security.

1. Open a terminal window in the path of your working folder.
2. At the prompt, type `python -m SimpleHTTPServer` to start a web server using Python. You should receive a message similar to this in the terminal: `Serving HTTP on 0.0.0.0 port 8000 ...`
3. Open your browser to `http://localhost:8000`. (Localhost is a shortcut hostname that a computer can use to refer to itself, and is not viewable by anyone else.) If you are having problems, you can instead try the command `python -m http.server 8000` (for use with Python 3).

If the step was successful, you should see a blank index page with your title showing in the browser tab.

## Add references to CSS and JavaScript files

Because you are working with several external cascading style sheet (CSS) and JavaScript files, you need to add references to them in your index.html file. These include style sheets and JavaScript files for Leaflet, Leaflet Routing Machine, and Valhalla. You will need to add these into the <head> and <body> sections of the index.html.

As you are working, it’s a good idea to save your edits and periodically reload the browser page. This helps you identify problems quicker and trace them back to your most recent changes. You can also monitor the terminal window for error messages. For example, seeing a 404 error in this walkthrough often means that the file cannot be found and you should make sure the paths in your HTML match the locations on disk.

When you are referencing local files, you will need to modify the path to reflect your working directory. For example, if your Valhalla files are in a folder called lrm-valhalla, the path would look like this: `<link rel="stylesheet" href="\lrm-valhalla\leaflet.routing.valhalla.css"/>`

1. In index.html, in the `<head>` section, add a reference to your Leaflet CSS file, substituting your root path to the leaflet folder. 
  ```html
  <link rel="stylesheet" href="leaflet.css"/>
  ```

2. In the `<head>` section, add reference to the Valhalla CSS file, substituting your path to the lrm-valhalla folder. This file can be used instead of the CSS file from the Leaflet Routing Machine plug-in because it contains all the LRM icons, as well as additional ones for Valhalla. 
  ```html
  <link rel="stylesheet" href="leaflet.routing.valhalla.css"/>
  ```
  
3. In the `<body>` section, add the Leaflet JavaScript file. 
  ```html
  <script src="leaflet.js"></script>
  ```
  
4. Add the Tangram JavaScript file, which is the rendering engine you will be using to draw the map. 
  ```html
  <script src="https://mapzen.com/tangram/tangram.min.js"></script>
  ```

5. Add the Leaflet Routing Machine JavaScript file, substituting your path to the leaflet-routing-machine folder. 
  ```html
  <script src="leaflet-routing-machine.js"></script>
  ```
  
6. Add the Valhalla JavaScript file, substituting your path to the lrm-valhalla folder. 
  ```html 
  <script src="lrm-valhalla.js"></script>
  ```

After adding these, your index.html file should look something like this. Note that JavaScript can be inserted in either the `<head>` or the `<body>`, but the bottom of the `<body>` may improve loading of the page. To make sure the scripts load in the proper order, you will need to add some code in the next steps.

```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>My Valhalla Map</title>
    <link rel="stylesheet" href="\leaflet\leaflet.css"/>
    <link rel="stylesheet" href="\lrm-valhalla\leaflet.routing.valhalla.css"/>
  </head>
  <body>
    <script src="\leaflet\leaflet.js"></script>
    <script src="https://mapzen.com/tangram/0.2/tangram.min.js"></script>
    <script src="\leaflet-routing-machine\leaflet-routing-machine.js"></script>
    <script src="\lrm-valhalla\lrm-valhalla.js"></script>
  </body>
  </html>
```

## Add a map to the page
To display a Leaflet map on a page, you need a `<div>` element with an ID value, as well as a size for the box containing the map. Then, you can add the code to set the map and use Tangram to render it. If you want to know more about initializing a Leaflet map, see the Leaflet getting started documentation.

With the script references in the `<body>`, it is possible that code could load before its dependencies. To resolve this, you can wrap the scripts in a function.

1. At the bottom of the `<head>` section, add a `<style>` tag and the following size attributes.
  ```html
  <style>
   #map {
     height: 100%;
     width: 100%;
     position: absolute;
    }
  </style>
  ```
  
2. At the top of the `<body>` section, add the `<div>`.
  ```html
  <div id="map"></div>
  ```
3. Immediately below the `<div>`, add the following JavaScript within a `<script>` tag to initialize Leaflet. You need to include the script within a function so it after any the JavaScript dependencies you referenced in the `<body>` tag.
  ```html
  <script>
  function loadMap(){
    var map = L.map('map');
  window.onload = loadMap;
  </script>
  ```
  
4. Save your edits and refresh the browser. You should see a blank canvas with zoom controls and an attribution in the corner.

Your index.html should look something like this:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Valhalla Map</title>
  <link rel="stylesheet" href="\leaflet\leaflet.css"/>
  <link rel="stylesheet" href="\lrm-valhalla\leaflet.routing.valhalla.css"/>
  <style>
  #map {
    height: 100%;
    width: 100%;
    position: absolute;
  }
  </style>
</head>
<body>
  <div id="map"></div>
  <script>
  function loadMap(){
    var map = L.map('map');
    layer.addTo(map);
  };
  window.onload = loadMap;
  </script>
  <script src="\leaflet\leaflet.js"></script>
  <script src="https://mapzen.com/tangram/0.2/tangram.min.js"></script>
  <script src="\leaflet-routing-machine\leaflet-routing-machine.js"></script>
  <script src="\lrm-valhalla\lrm-valhalla.js"></script>
</body>
</html>
```

## Add a Tangram map to the frame

At this point, you have enabled the basic Leaflet controls, but still need to tell Leaflet to use Tangram to draw the contents of the map.

Tangram uses a scene file in .yaml format to specify the what it should draw and how the features should appear in the map. A basic scene file has a reference to a data source (in this case, OpenStreetMap data from Mapzen’s vector tile service) and the colors and types of features to draw. In the code you will add, the `scene:` item sets the Tangram scene file to use for drawing and `attribution:` is what appears in the bottom corner of the map as the map attribution, overriding the default Leaflet attribution.

1. Inside the `<script>` tags, between the `var map = L.map('map');` and `layer.addTo(map);` lines, add the following code to use Tangram.
  ```html
  var layer = Tangram.leafletLayer({
    scene: 'scene.yaml',
    attribution: '<a href="https://mapzen.com/tangram" target="_blank">Tangram</a> | <a href="http://www.openstreetmap.org/about" target="_blank">&copy; OSM contributors | <a href="https://mapzen.com/" target="_blank">Mapzen</a>',
  });
  ```

2. After the Tangram section, add a line to initialize the map display. This sets the coordinates of the map and the zoom level.
  ```html
  map.setView([41.9067,-89.0688], 11);
  ```

3. Save your edits and refresh the browser. You should see Leaflet map controls and an updated attribution, and the map should be centered at the location specified.

Your `<script>` section of the `<body>` should look like this:
```html
<body>
  <div id="map"></div>
  <script>
  function loadMap(){
    var map = L.map('map');
    var layer = Tangram.leafletLayer({
      scene: 'scene.yaml',
      attribution: '<a href="https://mapzen.com/tangram" target="_blank">Tangram</a> | <a href="http://www.openstreetmap.org/about" target="_blank">&copy; OSM contributors | <a href="https://mapzen.com/" target="_blank">Mapzen</a>',
    });
    layer.addTo(map);
    map.setView([41.9067,-89.0688], 11);
  window.onload = loadMap;
  </script>

  [...]
</body>
```

## Add waypoints for routing
So far, you have referenced the necessary files, initialized Leaflet with a map container on the  page, and added Tangram to the map. Now, you are ready to add the routing code to your page using the Leaflet Routing Machine plug-in, using `L.Routing.control`.

In the simplest implementation, your map will not provide the ability to search for places through geocoding or inputting coordinates otherwise. Therefore, you need to set the waypoints in your code. As you add functionality to your web page, you can set the initial coordinates through user interaction.

1. Add // to comment out the line you added to set the extent. You no longer need this because the routing environment will be specifying it.  
  ```html
  //map.setView([41.9067,-89.0688], 11);
  ```
  
2. Inside the `<script>` tag, but below what you added in the previous section, initialize routing with the following code. You can substitute your own coordinates for the start and end locations of the routing.
  ```html
    L.Routing.control({
      waypoints: [
        L.latLng(41.9067,-89.0688),
        L.latLng(41.5893,-90.5715)
      ]
    }).addTo(map);
    ```
    
2. Save your edits and refresh the browser. You should see a map with the route displaying and a panel with narration.

## Set Valhalla as the routing engine

By default, the Leaflet Routing Machine plug-in uses the Open Source Routing Machine to perform the routing queries. To use a different engine, you need to set the `router:` as Valhalla and initialize a `formatter:`, which does not need a value because the Valhalla libraries already contain functions for units and other conversions.

The code you are given for `router:` has two items with placeholders. You need to replace these with your own API key and the routing mode you want to use in your map.

For this map, you will be able to drag the start and end points to update the routing, but the route will not be recalculated until you drop the points. You can set the option for `routeWhileDragging` to `true` if you want to update the route while moving points on the map, but this can be slow and costly to make many queries. By including a `summaryTemplatemplate`, the directions can include totals of the length and expected time en route. You can read more about the options available for `L.Routing.control` in the LRM API documentation.

1. In the `L.Routing.control` block, after the waypoints, add the following code to initialize Valhalla as the router.
  ```html
  router: L.Routing.valhalla('<my api key>', '<my routing mode>'),
  formatter: new L.Routing.Valhalla.Formatter(),
  summaryTemplate:'<div class="start">{name}</div><div class="info {transitmode}">{distance}, {time}</div>',
  routeWhileDragging: false
  ```
  
2. Go back to the https://mapzen.com/developers page and copy your API key to the clipboard.
3. Paste your own API key in place of `<my api key>` inside the single quotes.
4. Change `<my routing mode>` to `auto` to perform routing by automobile.
  ```html
  router: L.Routing.valhalla('valhalla-xxxxxx', 'auto'),
  ```

The routing section should look something like this, but with your own API key:
  ```html
  //map.setView([41.9067,-89.0688], 11);
  L.Routing.control({
    waypoints: [
      L.latLng(41.9067,-89.0688),
      L.latLng(41.5893,-90.5715)
    ],
    router: L.Routing.valhalla('valhalla-xxxxxx', 'auto'),
    formatter: new L.Routing.Valhalla.Formatter(),
    summaryTemplate:'<div class="start">{name}</div><div class="info {transitmode}">{distance}, {time}</div>',
    routeWhileDragging: false
  }).addTo(map);
 ```

When you refresh the map, you should see the map, route line, and updated icons and summary text in the narration box. You can change ‘auto’ to ‘pedestrian’ or ‘bicycle’ if you want to build a route for walking or biking, although this particular route would exceed Valhalla's limits for other modes of transport.

## Walkthrough summary and next steps
In this walkthrough, you learned the basics of making a map with Valhalla routing within the Leaflet and Leaflet Routing Machine frameworks. You can now take what you have learned and add more functionality to your map and embed it in your own projects. For example, you may want to add code to allow the user to pick routing locations with a button, change the mode of routing, or set other options for routing. You can also drag the route to change the start and end points or the points through which the route should pass.

Each of the routing modes that Valhalla supports has many options that can be used to influence the output route and estimated time. For example, automobile routing allows you to set penalties and costs to avoid toll roads or crossing international borders, and bicycle routing allows you to specify the category of bicycle so you are routed on appropriate paths for your equipment. You can review the documentation to learn more about costing options with Valhalla.
