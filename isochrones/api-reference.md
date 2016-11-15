# Isochrone service API reference

The Isochrone service provides a computation of areas that are reachable within specified time periods from a location or set of locations. Alternatively, isochrones can provide an estimation of the areas that can reach specific locations within specified time periods. This is generally valid for pedestrian routes, but for automobile and bicycle routes more work is needed. Isochrones are useful for visualizing service regions for specific locations on a map. To facilitate visualization, the Isochrone service returns the reachable regions as polygons or lines in GeoJSON. GeoJSON is readily ingested and drawn by many mapping applications and easily integrated into Web maps.

The Isochrone service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in the [Thor GitHub repository](https://github.com/valhalla/thor/issues) or send a message to [routing@mapzen.com](mailto:routing@mapzen.com).

The Mapzen Isochrone service requires an API key to use and has certain service limits. Learn more about getting started in the Mapzen developer overview documentation.

## Inputs of the isochrone service

A request takes the form of `matrix.mapzen.com/isochrone?json={}&api_key=`, where `isochrone?` indicates an isochrone is requested and the JSON inputs inside the ``{}`` include an array of at least one location and options for the route costing model.

For example, you want to know where you can travel within a 15-minute walk from your office building. The API request for this uses `isochrone?` as the request action, `pedestrian` costing, and a single contour for a 15-minute time interval. The response is GeoJSON, which you can display on a map to visualize where you might be able to walk.


    matrix.mapzen.com/isochrone?json={"locations":[{"lat":40.744014,"lon":-73.990508}],"costing":"pedestrian","contours":[{"time":15,"color":"ff0000"}]}&id=Walk_From_Office&api_key=matrix-xxxxxx

There is an option to name your isochrone request by appending `&id=`. The `id` is returned with the response so you can match to your corresponding request.

Note that you must append your own [API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.

### Location parameters

The `locations` must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. External search services, such as [Mapzen Search](https://mapzen.com/documentation/search/) can be used to find places and geocode addresses, whose coordinates can be used as input to the Isochrone service.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Refer to the [Turn-by-Turn location documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#locations) for more information on specifying locations.

### Costing parameters

The isochrone service uses the `auto`, `bicycle`, `pedestrian`, and `multimodal` costing models available in the Mapzen Turn-by-Turn service. Refer to the [Turn-by-Turn costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-models) and [costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-options) documentation for more on how to specify this input.

### Other request parameters

| Parameter | Description |
| :------------------ | :----------- |
| `date_time` | The local date and time at the location.<ul><li>`type`<ul><li>0 - Current departure time.</li><li>1 - Specified departure time</li><li>2 - Specified arrival time. Not yet implemented for multimodal costing method.</li></ul></li><li>`value` - the date and time is specified in ISO 8601 format (YYYY-MM-DDThh:mm) in the local time zone of departure or arrival.  For example "2016-07-03T08:06"</li></ul> |
| `id` | Name of your isochrone request. If `id` is specified, the naming will be sent through to the response. |
| `contours` | A JSON array of contour objects that specify the time and color to use for each isochrone contour. |
| `polygon` | A Boolean value to determine whether to return polygons or lines (as LineStrings) as the contours. The default is `false`, which returns LineStrings; when `true`, polygons are returned. **Note** polygonal output is equivalent returning any contour that is a ring as a polygon. For proper rendering (with opacity) more work is needed to turn demote some rings to inners of other rings and to remove potential self intersections. |
| `denoise` | A floating point value from `0` to `1` (defaulted to `1`) which can be used to remove smaller contours. A value of `1` will only return the largest contour. A value of `0.5` will drop any contours that are less than half the area of the largest contour in the set of contours for that same time value (see below). |
| `generalize` | A floating point value in meters used as the tolerance for [Douglas-Peucker](https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm) generalization. **Note** generalization of contours can lead to self-intersections as well as intersections of adjacent contours. |

##### Isochrone contours

A list of contours or isochrone polygons to generate from the specified locations. Each isochrone contour contains the following:

| Parameter | Description |
| :------------------ | :----------- |
| `time` | The time in minutes for the contour. |
| `color` | The color for the output of the contour. Specify it as a [Hex value](http://www.w3schools.com/colors/colors_hexadecimal.asp), but without the `#`, such as `"color":"ff0000"` for red. If no color is specified, the isochrone service will assign a default color to the output. |

## Outputs of the isochrone service

The isochrone contours are returned as GeoJSON, which can be integrated into mapping applications. You can learn more about GeoJSON and its specification [here](http://geojson.org/).

The contours are calculated using rasters and are returned as either polygon or line (as LinesStrings) vector features, depending on your input setting for the `polygon` parameter. If an isochrone request has been named using the optional `&id=` input, then the name will be returned as a name property for the feature collection within the GeoJSON response.

See the [HTTP return codes](turn-by-turn/api-reference/#return-codes-and-conditions) for more on messages you might receive from the service.

### Draw isochrones on a map

The isochrone styling information from the response can be used by most JavaScript-based GeoJSON renderers, including [Leaflet](http://leafletjs.com/). At present, you cannot control the opacity through the API.

When making a map, drawing the isochrone contours as lines is more straightforward than polygons, and, therefore, currently is the default and recommended method. When deciding between the output as lines and polygons, consider your use case and the additional styling considerations involved with polygons. For example, fills should be rendered as semi-transparent over the other map layers so they are visible, although you may have more flexibility when using a vector-based map. In addition, polygons from multiple contour levels do not have overlapping areas cut out or removed. In other words, the outer contours include the areas of any inner contours, causing the colors and transparencies to blend when multiple contour polygons are drawn at the same time.

Mapzen is working on improving degeneracies and self-intersections in polygon geometries to improve the rendering capabilities.

## Future work on the isochrone service

Several other options are being considered as future service enhancements. These include:

* using distance rather than time for each unit.
* generating outer contours or contours with interior holes for regions that cannot be accessed within the specified time, including with options to control the minimum size of interior holes.
* allowing multiple locations to compute the region reachable from any of the locations within a specified time.
* generating contours with reverse access logic to see the region that can reach a specific location within the specified time.
* returning raster data for potential animation using OpenGL shaders. This also has analysis use for being able to query thousands of locations to determine the time to each location, including improvements with one-to-many requests to the Mapzen Time-Distance Matrix service.

## Sample isochrone demonstration

Mobility Explorer helps you understand transportation networks around the world, and tools for adding isochrones to a map. Search for a location and add a point to the map, then generate isochrones for certain modes of transit from that location.

- Mobility Explorer: https://mapzen.com/mobility/explorer
- [documentation](explorer/overview.md)
