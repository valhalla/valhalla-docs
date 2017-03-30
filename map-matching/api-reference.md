
# Mapzen Map-Matching service API reference

Mapzen map-matching service is powered by the Valhalla engine. Valhalla is an open-source routing service that lets you integrate routing and navigation into a web or mobile application. This page documents the inputs and outputs to the map-matching service.

There are two separate map-matching service calls that perform different operations on an input set of latitude,longitude coordinates.

The first action is called trace_route. The trace_route action takes the mode and a list of latitude,longitude coordinates, for example from a GPS trace, and turns it into a route result. Sample use cases for trace_route include:
* Sharing a recorded route. Turn a recorded GPS trace into a route, complete with shape snapped to the road network and a set of guidance directions. An example is turning a GPS traces from a bike route into a set of narrative instructions.

The second action is called trace_attributes. The trace_attributes action takes the mode and a GPS trace or latitude, longitude positions from a portion of an existing route and returns detailed attribution along the portion of the route. This includes details for each section of road along the path as well as any intersections along the path. Sample use cases include:
* Just-in-time information for navigation. Returning full details along an entire route can create a very large payload. Regular route responses from Valhalla include shape and a set of maneuvers along each route leg. The maneuvers are a generalization of the path to simplify the description. Detailed attributes and localization of attributes along a maneuver would require significant additions to the route response. For long and even moderate length routes this can be wasteful, as the chances of re-routing along a long route are high. An alternate approach is to request detailed information “just-in-time” for portions of the upcoming route.
* Speed limits.  Speed limits along a path are a good example of just-in-time information that can be used for navigation. A single maneuver in a route (US-1 for example) may have many different speed limits along the full length of the maneuver. The trace_attributes action allows speed limits along each road segment to be determined and associated to portions of a maneuver.
* Obtaining way Ids. Another use case is to turn a GPS trace into a set of way Ids that match the trace. The trace_attributes action enables this.
* Finding the current road. A simple map-matching call with a recent set of GPS locations can be useful to find information about the current road, even if not doing navigation or have a route loaded on device. 

Note that the attributes that are returned are Valhalla routing attributes, not the base OSM tags or base data. Valhalla imports OSM tags and “normalizes” many of them to a standard set of values used for routing. Users interested in the base OSM tags along a path would need to take the OSM way IDs (returned as attributes along the path) and query OSM via a process like overpass to get the actual base data.

The map-matching service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in GitHub within the [valhalla repository](https://github.com/valhalla/valhalla). You can also send a message to routing@mapzen.com.

The default logic for the OpenStreetMap tags, keys, and values used when routing are documented on an [OSM wiki page](http://wiki.openstreetmap.org/wiki/OSM_tags_for_routing/Valhalla).


## Improving Map-Matching Results

* GPS accuracy is within TBD meters
* Trace point density is within the range of one per second and one per 10 seconds
* Each trace represents one continuous path
* Corresponding match with the OpenStreetMap network
* NOTE: Traces in urban areas will have a negative effect on GPS accuracy


## Criteria For Accurate Map-Match Results

* `turn_penalty_factor` - To penalize turns from one road segment to next.  For a pedestrian trace_route, you may see a back-and-forth on side streets for your path.  Try increasing the turn penalty factor to 500 to smooth out jittering of points. Note that if GPS accuracy is already good, increasing this will have a negative affect on your results.  
* `gps_accuracy` - GPS accuracy in meters.
* `search_radius` - To specify the search radius (in meters) within which to search road candidates for each measurement.  The `max_search_radius` is 100 so this can only be increased to 100.  Note that performance will decrease the higher the `search_radius`.

Example request:  <TODO: next checkin><add url here> "trace_options":{"turn_penalty_factor":500, "search_radius":55}


## Map-Matching Limitations

* `max_search_radius` - The maximum of the upper bounds of the search radius is 100 meters.
* `max_distance` - The maximum input shape distance is 200000.0 meters.
* `max_shape` - The maximum number of input shape points is 16000.


## Map-Matching Service Actions:

Our map-matching service includes two separate service calls that perform different operations on an input set of latitude, longitude coordinates, depending on your interest: `/trace_route?` or `/trace_attributes?`.  It is important to note that all service requests should be *POST* since shape or encoded_polyline can be fairly large.

*Map-Matching Action 1*  `trace_route`

*URL*

https://valhalla.mapzen.com/trace_route?api_key=

*POST Body*
```
{"encoded_polyline":"_grbgAh~{nhF?lBAzBFvBHxBEtBKdB?fB@dBZdBb@hBh@jBb@x@\\|@x@pB\\x@v@hBl@nBPbCXtBn@|@z@ZbAEbAa@~@q@z@QhA]pAUpAVhAPlAWtASpAAdA[dASdAQhAIlARjANnAZhAf@n@`A?lB^nCRbA\\xB`@vBf@tBTbCFbARzBZvBThBRnBNrBP`CHbCF`CNdCb@vBX`ARlAJfADhA@dAFdAP`AR`Ah@hBd@bBl@rBV|B?vB]tBCvBBhAF`CFnBXtAVxAVpAVtAb@|AZ`Bd@~BJfA@fAHdADhADhABjAGzAInAAjAB|BNbCR|BTjBZtB`@lBh@lB\\|Bl@rBXtBN`Al@g@t@?nAA~AKvACvAAlAMdAU`Ac@hAShAI`AJ`AIdAi@bAu@|@k@p@]p@a@bAc@z@g@~@Ot@Bz@f@X`BFtBXdCLbAf@zBh@fBb@xAb@nATjAKjAW`BI|AEpAHjAPdAAfAGdAFjAv@p@XlAVnA?~A?jAInAPtAVxAXnAf@tBDpBJpBXhBJfBDpAZ|Ax@pAz@h@~@lA|@bAnAd@hAj@tAR~AKxAc@xAShA]hAIdAAjA]~A[v@BhB?dBSv@Ct@CvAI~@Oz@Pv@dAz@lAj@~A^`B^|AXvAVpAXdBh@~Ap@fCh@hB\\zBN`Aj@xBFdA@jALbAPbAJdAHdAJbAHbAHfAJhALbA\\lBTvBAdC@bC@jCKjASbC?`CM`CDpB\\xAj@tB\\fA\\bAVfAJdAJbAXz@L|BO`AOdCDdA@~B\\z@l@v@l@v@l@r@j@t@b@x@b@r@z@jBVfCJdAJdANbCPfCF|BRhBS~BS`AYbAe@~BQdA","shape_match":"map_snap","costing":"pedestrian","directions_options":{"units":"miles"}}
```

*Map-Matching Action 2*  `trace_attributes`

*URL*

https://valhalla.mapzen.com/trace_attributes?api_key=

*POST Body*
```
{"encoded_polyline":"_grbgAh~{nhF?lBAzBFvBHxBEtBKdB?fB@dBZdBb@hBh@jBb@x@\\|@x@pB\\x@v@hBl@nBPbCXtBn@|@z@ZbAEbAa@~@q@z@QhA]pAUpAVhAPlAWtASpAAdA[dASdAQhAIlARjANnAZhAf@n@`A?lB^nCRbA\\xB`@vBf@tBTbCFbARzBZvBThBRnBNrBP`CHbCF`CNdCb@vBX`ARlAJfADhA@dAFdAP`AR`Ah@hBd@bBl@rBV|B?vB]tBCvBBhAF`CFnBXtAVxAVpAVtAb@|AZ`Bd@~BJfA@fAHdADhADhABjAGzAInAAjAB|BNbCR|BTjBZtB`@lBh@lB\\|Bl@rBXtBN`Al@g@t@?nAA~AKvACvAAlAMdAU`Ac@hAShAI`AJ`AIdAi@bAu@|@k@p@]p@a@bAc@z@g@~@Ot@Bz@f@X`BFtBXdCLbAf@zBh@fBb@xAb@nATjAKjAW`BI|AEpAHjAPdAAfAGdAFjAv@p@XlAVnA?~A?jAInAPtAVxAXnAf@tBDpBJpBXhBJfBDpAZ|Ax@pAz@h@~@lA|@bAnAd@hAj@tAR~AKxAc@xAShA]hAIdAAjA]~A[v@BhB?dBSv@Ct@CvAI~@Oz@Pv@dAz@lAj@~A^`B^|AXvAVpAXdBh@~Ap@fCh@hB\\zBN`Aj@xBFdA@jALbAPbAJdAHdAJbAHbAHfAJhALbA\\lBTvBAdC@bC@jCKjASbC?`CM`CDpB\\xAj@tB\\fA\\bAVfAJdAJbAXz@L|BO`AOdCDdA@~B\\z@l@v@l@v@l@r@j@t@b@x@b@r@z@jBVfCJdAJdANbCPfCF|BRhBS~BS`AYbAe@~BQdA","shape_match":"map_snap","costing":"pedestrian","directions_options":{"units":"miles"}}
```

## Inputs to map-matching

Example request: 
http://localhost:8002/trace_attributes?json={"shape":[{"lat":39.983841,"lon":-76.735741},{"lat":39.983704,"lon":-76.735298},{"lat":39.983578,"lon":-76.734848},{"lat":39.983551,"lon":-76.734253},{"lat":39.983555,"lon":-76.734116},{"lat":39.983589,"lon":-76.733315},{"lat":39.983719,"lon":-76.732445},{"lat":39.983818,"lon":-76.731712},{"lat":39.983776,"lon":-76.731506},{"lat":39.983696,"lon":-76.731369}],"costing":"auto","shape_match":"walk_or_snap","filters":{"attributes":["edge.names","edge.id", "edge.weighted_grade","edge.speed"],"action":"include"}}

`shape_match` is an optional string input parameter. It allows some control of the matching algorithm based on the type of input. 

| `shape_match` type | Description |
| :--------- | :----------- |
| `edge_walk` | Indicates an edge walking algorithm can be used. This algorithm requires nearly exact shape matching so it should only be used when the shape is from a prior Valhalla route. |
| `map_snap` | Indicates that a map matching algorithm should be used since the input shape might not closely match Valhalla edges. This algorithm is more expensive. |
| `walk_or_snap` | Also the default options. This will try edge walking and if this does not succeed it will fall-back and use map-matching. |

Note that you must append your own [API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.


### Costing models

Mapzen Turn-by-Turn uses dynamic, run-time costing to generate the route path. The route request must include the name of the costing model and can include optional parameters available for the chosen costing model.

| Costing model | Description |
| :----------------- | :----------- |
| `auto` | Standard costing for driving routes by car, motorcycle, truck, and so on that obeys automobile driving rules, such as access and turn restrictions. `Auto` provides a short time path (though not guaranteed to be shortest time) and uses intersection costing to minimize turns and maneuvers or road name changes. Routes also tend to favor highways and higher classification roads, such as motorways and trunks.  Here is an example auto route request at the current date and time: `http://valhalla.mapzen.com/route?json={"locations":[{"lat":40.730930,"lon":-73.991379,"street":"Wanamaker Place"},{"lat":40.749706,"lon":-73.991562,"street":"Penn Plaza"}],"costing":"auto","directions_options":{"units":"miles"}}&api_key=mapzen-xxxxxxx`  Note that you must append your own [Mapzen API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end. |
| `auto_shorter` | Alternate costing for driving that provides a short path (though not guaranteed to be shortest distance) that obeys driving rules for access and turn restrictions. |
| `bicycle` | Standard costing for travel by bicycle, with a slight preference for using [cycleways](http://wiki.openstreetmap.org/wiki/Key:cycleway) or roads with bicycle lanes. Bicycle routes follow regular roads when needed, but avoid roads without bicycle access. |
| `bus` | Standard costing for bus routes. Bus costing inherits the auto costing behaviors, but checks for bus access on the roads. |
| `pedestrian` | Standard walking route that excludes roads without pedestrian access. In general, pedestrian routes are shortest distance with the following exceptions: walkways and footpaths are slightly favored, while steps or stairs and alleys are slightly avoided. |
| `multimodal` | We do NOT support `multimodal` for map-matching since it would be difficult to get favorable gps traces. |


#### Directions options

| Options | Description |
| :------------------ | :----------- |
| `units` | Distance units for output. Allowable unit types are miles (or mi) and kilometers (or km). If no unit type is specified, the units default to kilometers. |
| `language` | The language of the narration instructions based on the [IETF BCP 47](https://tools.ietf.org/html/bcp47) language tag string. If no language is specified or the specified language is unsupported, United States-based English (en-US) is used. Currently supported language tags with alias in parentheses:  cs-CZ (cs), de-DE (de), en-US (en), en-US-x-pirate (pirate), es-ES (es), fr-FR (fr), hi-IN (hi), it-IT (it). |
| `narrative` |  Boolean to allow you to disable narrative production. Locations, shape, length, and time are still returned. The narrative production is enabled by default. Set the value to `false` to disable the narrative. |


#### Attribute filters

The trace_attribues api allows you to apply filters to `include` or `exclude` specific attribute filter keys in your response.  These filters are optional and can be added to the action string inside of the filters object.  The available list of filter keys are listed below.

Example to include attribute filters:
http://localhost:8002/trace_attributes?json={"shape":[{"lat":39.983841,"lon":-76.735741},{"lat":39.983704,"lon":-76.735298},{"lat":39.983578,"lon":-76.734848},{"lat":39.983551,"lon":-76.734253},{"lat":39.983555,"lon":-76.734116},{"lat":39.983589,"lon":-76.733315},{"lat":39.983719,"lon":-76.732445},{"lat":39.983818,"lon":-76.731712},{"lat":39.983776,"lon":-76.731506},{"lat":39.983696,"lon":-76.731369}],"costing":"auto","shape_match":"walk_or_snap","filters":{"attributes":["edge.names","edge.id", "edge.weighted_grade","edge.speed"],"action":"include"}}

Example to exclude attribute filters:
http://localhost:8002/trace_attributes?json={"shape":[{"lat":39.983841,"lon":-76.735741},{"lat":39.983704,"lon":-76.735298},{"lat":39.983578,"lon":-76.734848},{"lat":39.983551,"lon":-76.734253},{"lat":39.983555,"lon":-76.734116},{"lat":39.983589,"lon":-76.733315},{"lat":39.983719,"lon":-76.732445},{"lat":39.983818,"lon":-76.731712},{"lat":39.983776,"lon":-76.731506},{"lat":39.983696,"lon":-76.731369}],"costing":"auto","shape_match":"walk_or_snap","filters":{"attributes":["edge.names","edge.begin_shape_index","edge.end_shape_index","shape"],"action":"exclude"}}

If no filters are used then we enable and return all attributes in the the trace_attributes response.

Below is a list of filter keys - please find [descriptions](#outputs-of-trace_attributes) below.
```
// Edge Filter Keys
edge.names
edge.length
edge.speed
edge.road_class
edge.begin_heading
edge.end_heading
edge.begin_shape_index
edge.end_shape_index
edge.traversability
edge.use
edge.toll
edge.unpaved
edge.tunnel
edge.bridge
edge.roundabout
edge.internal_intersection
edge.drive_on_right
edge.surface
edge.sign.exit_number
edge.sign.exit_branch
edge.sign.exit_toward
edge.sign.exit_name
edge.travel_mode
edge.vehicle_type
edge.pedestrian_type
edge.bicycle_type
edge.transit_type
edge.id
edge.way_id
edge.weighted_grade
edge.max_upward_grade
edge.max_downward_grade
edge.lane_count
edge.cycle_lane
edge.bicycle_network
edge.sidewalk
edge.density
edge.speed_limit
edge.truck_speed
edge.truck_route

// Node Filter Keys
node.intersecting_edge.begin_heading
node.intersecting_edge.from_edge_name_consistency
node.intersecting_edge.to_edge_name_consistency
node.intersecting_edge.driveability
node.intersecting_edge.cyclability
node.intersecting_edge.walkability
node.elapsed_time
node.admin_index
node.type
node.fork
node.time_zone

// Other Filter Keys
osm_changeset
shape
admin.country_code
admin.country_text
admin.state_code
admin.state_text
```

## Outputs of trace_route

The outputs of the trace_route action are the same as the [outputs of a route](https://mapzen.com/documentation/mobility/turn-by-turn/api-reference/#outputs-of-a-route) action.

## Outputs of trace_attributes
The `trace_attributes` results contains a list edges and optionally, the following items: osm_changeset, list of admins, shape, and units.

| Result Item | Description |
| :--------- | :---------- |
| `edges` | List of edges associated with input shape. See below for details. |
| `osm_changeset` | Base data version identifier. |
| `admins` | List of the administrative codes and names. |
| `shape` | The [encoded polyline](https://developers.google.com/maps/documentation/utilities/polylinealgorithm) of the matched path. |
| `units` | The specified units with the request - `kilometers` or `miles`. |

Each `edge` may include:

| Edge Item | Description |
| :--------- | :---------- |
| `names` | List of names. |
| `length` | Edge length in the units specified. The default is kilometers. |
| `speed` | Edge speed in the units specified. The default is kph. |
| `road_class` | Road class values:<ul><li>`motorway`</li><li>`trunk`</li><li>`primary`</li><li>`secondary`</li><li>`tertiary`</li><li>`unclassified`</li><li>`residential`</li><li>`service_other`</li></ul> |
| `begin_heading` | The direction at the beginning of an edge. The units are degrees from north in a clockwise direction. |
| `end_heading` | The direction at the end of an edge. The units are degrees from north in a clockwise direction.. |
| `begin_shape_index` | Index into the list of shape points for the start of the edge. |
| `end_shape_index` | Index into the list of shape points for the end of the edge. |
| `traversability` | Traversability values, if available:<ul><li>`forward`</li><li>`backward`</li><li>`both`</li></ul> |
| `use` | Use values: <ul><li>`tram`</li><li>`road`</li><li>`ramp`</li><li>`turn_channel`</li><li>`track`</li><li>`driveway`</li><li>`alley`</li><li>`parking_aisle`</li><li>`emergency_access`</li><li>`drive_through`</li><li>`culdesac`</li><li>`cycleway`</li><li>`mountain_bike`</li><li>`sidewalk`</li><li>`footway`</li><li>`steps`</li><li>`other`</li><li>`rail-ferry`</li><li>`ferry`</li><li>`rail`</li><li>`bus`</li><li>`rail_connection`</li><li>`bus_connnection`</li><li>`transit_connection`</li></ul> |
| `toll` | True if the edge has any toll. |
| `unpaved` | True if the edge is unpaved or rough pavement. |
| `tunnel` | True if the edge is a tunnel. |
| `bridge` | True if the edge is a bridge. |
| `roundabout` | True if the edge is a roundabout. |
| `internal_intersection` | True if the edge is an internal intersection. |
| `drive_on_right` | True if the drive on the right side of the street flag is endabled. |
| `surface` | Surface values: <ul><li>`paved_smooth`</li><li>`paved`</li><li>`paved_rough`</li><li>`compacted`</li><li>`dirt`</li><li>`gravel`</li><li>`path`</li><li>`impassable`</li></ul> |
| `sign` | Contains the interchange guide information associated with this edge. See below for details. |
| `travel_mode` | Travel mode values:<ul><li>`drive`</li><li>`pedestrian`</li><li>`bicycle`</li><li>`transit`</li></ul> |
| `vehicle_type` | Vehicle type values:<ul><li>`car`</li><li>`motorcycle`</li><li>`bus`</li><li>`tractor_trailer`</li></ul> |
| `pedestrian_type` | Pedestrian type values:<ul><li>`foot`</li><li>`wheelchair`</li><li>`segway`</li></ul> |
| `bicycle_type` | Bicycle type values:<ul><li>`road`</li><li>`cross`</li><li>`hybrid`</li><li>`mountain`</li></ul> |
| `transit_type` | Transit type values: <ul><li>`tram`</li><li>`metro`</li><li>`rail`</li><li>`bus`</li><li>`ferry`</li><li>`cable_car`</li><li>`gondola`</li><li>`funicular`</li></ul>|
| `id` | Identifier of an edge within the tiled, hierarchical graph. |
| `way_id` | Way identifier of the base data. |
| `weighted_grade` | The weighted grade factor. |
| `max_upward_grade` | The maximum upward slope. |
| `max_downward_grade` | The maximum downward slope. |
| `lane_count` | The number of lanes for this edge. |
| `cycle_lane` | The type (if any) of bicycle lane along this edge. |
| `bicycle_network` | The bike network for this edge. |
| `sidewalk` | Sidewalk values:<ul><li>`left`</li><li>`right`</li><li>`both`</li></ul> |
| `density` | The relative density along the edge. |
| `speed_limit` | Edge speed limit in the units specified. The default is kph. |
| `truck_speed` | Edge truck speed in the units specified. The default is kph. |
| `truck_route` | True if edge is part of a truck network/route. |
| `end_node` | The node at the end of this edge. See below for details. |

Each `sign` may include:

| Sign Item | Description |
| :--------- | :---------- |
| `exit_number` | List of exit number elements. If an exit number element exists, it is typically just one value. Element example: `91B` |
| `exit_branch` | List of exit branch elements. The exit branch element is the subsequent road name or route number after the sign. Element example: `I 95 North` |
| `exit_toward` | List of exit toward elements. The exit toward element is the location where the road ahead goes - the location is typically a control city, but may also be a future road name or route number. Element example: `New York` |
| `exit_name` | List of exit name elements. The exit name element is the interchange identifier - typically not used in the US. Element example: `Gettysburg Pike` |

Each `end_node` may include:

| Node Item | Description |
| :--------- | :---------- |
| `intersecting_edges` | List of intersecting edges at this node. See below for details. |
| `elapsed_time` | Elapsed time of the path to arrive at this node. |
| `admin_index` | Index into the admin list. |
| `type` | Node type values: <ul><li>`street_intersection`</li><li>`gate`</li><li>`bollard`</li><li>`toll_booth`</li><li>`multi_use_transit_stop`</li><li>`bike_share`</li><li>`parking`</li><li>`motor_way_junction`</li><li>`border_control`</li></ul> |
| `fork` | True if this node is a fork. |
| `time_zone` | Time zone string for this node. |

Each `intersecting_edge` may include:

| Intersecting Edge Item | Description |
| :--------- | :---------- |
| `begin_heading` | The direction at the beginning of this intersecting edge. The units are degrees from north in a clockwise direction. |
| `from_edge_name_consistency` | True if this intersecting edge at the end node has consistent names with the path `from edge`. |
| `to_edge_name_consistency` | True if this intersecting edge at the end node has consistent names with the path `to edge`. |
| `driveability` | Driveability values, if available:<ul><li>`forward`</li><li>`backward`</li><li>`both`</li></ul> |
| `cyclability` | Cyclability values, if available:<ul><li>`forward`</li><li>`backward`</li><li>`both`</li></ul> |
| `walkability` | Walkability values, if available:<ul><li>`forward`</li><li>`backward`</li><li>`both`</li></ul> |

Each `admin` may include:

| Admin Item | Description |
| :--------- | :---------- |
| `country_code` | Country [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code. |
| `country_text` | Country name. |
| `state_code` | State code. |
| `state_text` | State name. |

