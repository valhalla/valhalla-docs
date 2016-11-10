# Isochrones Service API reference

The Isochrone service provides a computation of areas that are reachable within specified time periods from a location or set of locations. Alternatively, isochrones can provide an estimation of the areas that can reach specific locations within specified time periods. This is generally valid for pedestrian routes, but for automobile and bicycle routes more work is needed. Isochrones are usful for visualizing service regions for specific locations on a map. To facilitate visualization, the Isochrone service returns the reachable regions as lines or polygons in GeoJSON. GeoJSON is readily ingested and drawn by many mapping applications and easily integrated into Web maps.

The Isochrone service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in the [Thor GitHub repository](https://github.com/valhalla/thor/issues) or send a message to [routing@mapzen.com](mailto:routing@mapzen.com).

## API keys and service limits

To use the Isochrone service, you must first obtain an API key from Mapzen. The isochrone service is part of Mapzen's matrix service. Sign in at https://mapzen.com/developers to create and manage your API keys.

As a shared service, there are limitations on requests, maximum time and number of isochrone contours, and numbers of locations. This prevents individual users from degrading the overall system performance.

The following limitations are currently in place.

* `max_locations` is  1. A call to the isochrone service supports a single location only at this time. To support isochrones about multiple locations, an application must make multiple requests.
* `max_time` is the maximum time from the location to compute isochrone contours. This is limited to 2 hours.
* `max_contours` is the maximum number of icochrone contours allowed in a single request. This is limited to 4 contours.
* Rate limits are two queries per second and 5,000 queries per day.

If you need more capacity, contact [routing@mapzen.com](mailto:routing@mapzen.com). You can also set up your own instance of Valhalla, which has access to functions similar to Mapzen's services.

## Inputs of the Isochrone service

An example request takes the form of `matrix.mapzen.com/isochrone?json={}&api_key=`, where `isochrone?` indicates an isochrone is requested and the JSON inputs inside the ``{}`` include an array of at least one location and options for the route costing model.

For example, while at your office, you want to know where you can reach within a 15 minute walk. The API request for this uses `isochrone?` as the request action and uses `pedestrian` costing. A single contour is requested at a 15 minute time interval. This will return a GeoJSON response that can easily be viewed on a map so you can visualize where you might be able to walk.

    matrix.mapzen.com/isochrone?json={"locations":[{"lat":40.744014,"lon":-73.990508}],"costing":"pedestrian","contours":[{"time":15,"color":TBD}]}&id=Walk_From_Office&api_key=matrix-xxxxxx

There is an option to name your isochrone request.  You can do this by appending the following to your request `&id=`.  The `id` is returned with the response so a user could match to the corresponding request.

Note that you must append your own [API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.

### Location parameters

A location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. External search services, such as [Mapzen Search](https://mapzen.com/documentation/search/) can be used to find places and geocode addresses, whose coordinates can be used as input to the Isochrone service.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Refer to the [Turn-by-Turn location documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#locations) for more information on specifying locations.

### Costing parameters

The Isochrone service uses the `auto`, `bicycle`, `pedestrian` and `multimodal` costing models available in the Mapzen Turn-by-Turn service. Refer to the [Turn-by-Turn costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-models) and [costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-options) documentation for more on how to specify this input.

### Other request parameters

| Parameter | Description |
| :------------------ | :----------- |
| `date_time` | This is the local date and time at the location.<ul><li>`type`<ul><li>0 - Current departure time.</li><li>1 - Specified departure time</li><li>2 - Specified arrival time. Not yet implemented for multimodal costing method.</li></ul></li><li>`value` - the date and time is specified in ISO 8601 format (YYYY-MM-DDThh:mm) in the local time zone of departure or arrival.  For example "2016-07-03T08:06"</li></ul> |
| `id` | Name your isochrone request. If `id` is specified, the naming will be sent through to the response. |
| `contours` | This is a JSON array of contour objects that specify the time and color to use for each isochrone contour. The number of contours requested must not exceed the `max_contours` allowed. |
| `polygon` | Boolean value (defaulted to `false`) that, when `true`, will only return polygonal contours dropping the linestring contours. **Note** polygonal output is simply equivallent returning any contour that is a ring as a polygon. For proper rendering (with opacity) more work is needed to turn demote some rings to inners of other rings and to remove potential self intersections. |
| `denoise` | A floating point value from `0` to `1` (defaulted to `1`) which can be used to remove smaller contours. A value of `1` will only return the largest contour. A value of `0.5` will drop any contours that are less than half the area of the largest contour in the set of contours for that same time value (see below). |
| `generalize` | A floating point value in meters used a the tolerance for Douglas Peucker generalization. **Note** generalization of contours can lead to self intersections as well as intersections of adjacent contours. |

#####Isochrone Contours
A list of contours or isochrone polygons to generate from the specified locations. Each isochrone contour contains the following:

| Parameter | Description |
| :------------------ | :----------- |
| `time` | This is the time in minutes for the contour. The value must not exceed the `max_time` allowed. |
| `color` | This is the color to use for the output of the contour polygon. If no color is specified, the isochrone service will assign a default color to the output. |

## Outputs of the Isochrone service

The isochrone contours are returned as GeoJSON. GeoJSON can easily be integrated into mapping applications. You can learn more about GeoJSON and its specification [here](http://geojson.org/).

At present the GeoJSON will contain linestring or polygon features. By default only linestrings will be returned however you can get polygons using the `polygon` parameter as described above. In addition to the geometry the properties of each feature will include styling information that most javascript based GeoJSON renderers will respect (especially leaflet). At present the opacity portion of the style is not controllable via the API.

If an isochrone request has been named using the optional `&id=` input, then the name will be returned as a name property for the feature collection within the GeoJSON response.

## Rendering Isochrones

By far the most common rendering of isochrones has to be raster based. This presents a bit of a problem in that you don't have much control over what appears in the raster (each pixel) or you've got to convert the raster to something you can display in the style you'd like to display it. So although internally we do indeed compute isochrones using a raster datatype we currently only return vectorized data.

As mentioned above we currently support linestring and polygon primative types. Rendering linestring data is quite straight forward and can be seen in our demo linked below. This is the prefered method for now as there are quite a few caveats one must deal with when rendering isochrones as polygons.

Rendering polygons is tricky business. When working with isochrones its generally useful to see them in the presence of a map. So to be able to see the map one must render the polygons as semi-transparent (unless you've got a vector based map, ie tangram, then you could render the map over the isochrone polygons). Because isochrones are actually contours, the polygons returned will be necessarily, nested. This nesting causes overlapping transparent polygons, which leaves you with portions of the map that have blended colors from two different isochrones and even worse varying amounts of transparency. To solve this we need a couple of things. We need the geometry of the polygons to be free of degeneracies, we'll say self intersection is a degeneracy and we need holes in polygons to be represented as such. These two gaurantees are currently not implemented, we're working on it.

Consider your use-case and use your best judgement to determine if rendering isochrones as linestrings or polygons is right for you!

## Future Work

Several other options are being considered as future service enhancements. These include:
* The ability to use distance rather than time for each unit.
* Optionally specify whether to generate outer contours only or contours with interior holes (regions that cannot be accessed within the specified time). Add options to control the minimum size of interior holes.
* Allowing multiple locations in order to compute the region reachable from any of the locations within a specified time.
* Generation of contours using "reverse" access logic to see the region that can reach a specific location within the specified time.
* Return of the raster data for potential animation using OpenGL shaders. This also has analysis use for being able to query thousands of locations to determine the time to each location. Adding distance to the raster cells would improve its use for large time-distance one to many matrix requests. The raster itself can be quite large so in terms of formats we may want to offset encode it and return the dimensions or write it to a binary 2 channel image format (useful for textures).

If you have comments or requests for any of these options (and others) please let us know.

See the [HTTP return codes](https://mapzen.com/documentation/turn-by-turn/api-reference/#return-codes-and-conditions) for more on messages you might receive from the service.

## Sample Isochrone Demonstration

If you want to see the results of the Isochrone service, a quick and easy way is using the [demo](https://valhalla.github.io/demos/isochrone/). Instructions TBD.
