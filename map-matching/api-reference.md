
# Mapzen Map-Matching service API reference

Mapzen map-matching service is powered by the Valhalla engine. Valhalla is an open-source routing service that lets you integrate routing and navigation into a web or mobile application. This page documents the inputs and outputs to the map-matching service.

There are two separate map-matching service calls that perform different operations on an input set of latitude,longitude coordinates.

The first action is called trace_route. The trace_route action takes a list of latitude,longitude coordinates, for example from a GPS trace, and turns it into a route result. Sample use cases for trace_route include:
* Sharing a recorded route. Turn a recorded GPS trace into a route, complete with shape snapped to the road network and a set of guidance directions. An example is turning a GPS traces from a bike route into a set of narrative instructions.
* Build-a-route. A rough set of sequential locations can be provided and an initial route can be constructed. This may yield a better result than using the rough locations as route locations, since the route would likely be forced through the locations while th

The second action is called trace_attributes. The trace_attributes action takes a GPS trace or latitude, longitude positions from a portion of an existing route and returns detailed attribution along the portion of the route. This includes details for each section of road along the path as well as any intersections along the path. Sample use cases include:
* Just-in-time information for navigation. Returning full details along an entire route can create a very large payload. Regular route responses from Valhalla include shape and a set of maneuvers along each route leg. The maneuvers are a generalization of the path to simplify the description. Detailed attributes and localization of attributes along a maneuver would require significant additions to the route response. For long and even moderate length routes this can be wasteful, as the chances of re-routing along a long route are high. An alternate approach is to request detailed information “just-in-time” for portions of the upcoming route.
* Speed limits.  Speed limits along a path are a good example of just-in-time information that can be used for navigation. A single maneuver in a route (US-1 for example) may have many different speed limits along the full length of the maneuver. The trace_attributes action allows speed limits along each road segment to be determined and associated to portions of a maneuver.
* Obtaining way Ids. Another use case is to turn a GPS trace into a set of way Ids that match the trace. The trace_attributes action enables this.
* Finding the current road. A simple map-matching call with a recent set of GPS locations can be useful to find information about the current road, even if not doing navigation or have a route loaded on device. 

Note that the attributes that are returned are Valhalla routing attributes, not the base OSM tags or base data. Valhalla imports OSM tags and “normalizes” many of them to a standard set of values used for routing. Users interested in the base OSM tags along a path would need to take the OSM way IDs (returned as attributes along the path) and query OSM via a process like overpass to get the actual base data.

The map-matching service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in GitHub within the [valhalla repository](https://github.com/valhalla/valhalla). You can also send a message to routing@mapzen.com.

The default logic for the OpenStreetMap tags, keys, and values used when routing are documented on an [OSM wiki page](http://wiki.openstreetmap.org/wiki/OSM_tags_for_routing/Valhalla).


## Inputs to map-matching

Example request:
http://localhost:8002/trace_attributes?json={"shape":[{"lat":39.983841,"lon":-76.735741},{"lat":39.983704,"lon":-76.735298},{"lat":39.983578,"lon":-76.734848},{"lat":39.983551,"lon":-76.734253},{"lat":39.983555,"lon":-76.734116},{"lat":39.983589,"lon":-76.733315},{"lat":39.983719,"lon":-76.732445},{"lat":39.983818,"lon":-76.731712},{"lat":39.983776,"lon":-76.731506},{"lat":39.983696,"lon":-76.731369}],"costing":"auto","shape_match":"walk_or_snap","filters":{"attributes":["edge.names","edge.id", "edge.weighted_grade","edge.speed"],"action":"only"}}

shape_match is an optional input parameter. It allows some control of the matching algorithm based on the type of input. If the input shape is from a prior Valhalla route request, the shape_match parameter can be set to "walk" which indicates an edge walking algorithm can be used. This algorithm requires nearly exact shape matching so it should only be used when the shape is from a prior Valhalla route. A shape_match set to walk indicates that a map matching algorithm should be used since the input shape might not closely match Valhalla edges. This algorithm is more expensive. The default option is "walk_or_snap", This will try edge walking and if this does not succeed it will fall-back and use map-matching.

Note that you must append your own [API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.


### Costing models

Mapzen Turn-by-Turn uses dynamic, run-time costing to generate the route path. The route request must include the name of the costing model and can include optional parameters available for the chosen costing model.

| Costing model | Description |
| :----------------- | :----------- |
| `auto` | Standard costing for driving routes by car, motorcycle, truck, and so on that obeys automobile driving rules, such as access and turn restrictions. `Auto` provides a short time path (though not guaranteed to be shortest time) and uses intersection costing to minimize turns and maneuvers or road name changes. Routes also tend to favor highways and higher classification roads, such as motorways and trunks. |
| `auto_shorter` | Alternate costing for driving that provides a short path (though not guaranteed to be shortest distance) that obeys driving rules for access and turn restrictions. |
| `bicycle` | Standard costing for travel by bicycle, with a slight preference for using [cycleways](http://wiki.openstreetmap.org/wiki/Key:cycleway) or roads with bicycle lanes. Bicycle routes follow regular roads when needed, but avoid roads without bicycle access. |
| `bus` | Standard costing for bus routes. Bus costing inherits the auto costing behaviors, but checks for bus access on the roads. |
| `multimodal` | Currently supports pedestrian and transit. In the future, multimodal will support a combination of all of the above.  Here is an example multimodal request at the current date and time: `http://valhalla.mapzen.com/route?json={"locations":[{"lat":40.730930,"lon":-73.991379,"street":"Wanamaker Place"},{"lat":40.749706,"lon":-73.991562,"street":"Penn Plaza"}],"costing":"multimodal","directions_options":{"units":"miles"}}&api_key=mapzen-xxxxxxx`  Note that you must append your own [Mapzen API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end. |
| `pedestrian` | Standard walking route that excludes roads without pedestrian access. In general, pedestrian routes are shortest distance with the following exceptions: walkways and footpaths are slightly favored, while steps or stairs and alleys are slightly avoided. |


#### Directions options

| Options | Description |
| :------------------ | :----------- |
| `units` | Distance units for output. Allowable unit types are miles (or mi) and kilometers (or km). If no unit type is specified, the units default to kilometers. |
| `language` | The language of the narration instructions based on the [IETF BCP 47](https://tools.ietf.org/html/bcp47) language tag string. If no language is specified or the specified language is unsupported, United States-based English (en-US) is used. Currently supported language tags with alias in parentheses:  cs-CZ (cs), de-DE (de), en-US (en), en-US-x-pirate (pirate), es-ES (es), fr-FR (fr), hi-IN (hi), it-IT (it). |
| `narrative` |  Boolean to allow you to disable narrative production. Locations, shape, length, and time are still returned. The narrative production is enabled by default. Set the value to `false` to disable the narrative. |

#### Attribute filters

| Attribute Key | Description |
| :------------------ | :----------- |
TODO

## Outputs of trace_route

The outputs of the trace_route action are the same as the [outputs of a route](https://mapzen.com/documentation/mobility/turn-by-turn/api-reference/#outputs-of-a-route) action.

## Outputs of trace_attributes
The `trace_attributes` results contains a list edges and optionally, the following items: osm_changeset, list of admins, shape, and units.

| Result Item | Description |
| :--------- | :---------- |
| `edges` | List of edges associated with input shape. |
| `osm_changeset` | TODO. |
| `admins` | TODO. |
| `shape` | TODO. |
| `units` | TODO. |

Each `edge` includes:

| Edge Item | Description |
| :--------- | :---------- |
| `names` | List of names. |
| `length` | Edge length in the units specified. The default is kilometers. |
| `speed` | Edge speed in the units specified. The default is kph. |
| `road_class` | TODO. |
| `begin_heading` | The direction at the beginning of an edge. The units are degrees from north in a clockwise direction. |
| `end_heading` | The direction at the end of an edge. The units are degrees from north in a clockwise direction.. |
| `begin_shape_index` | Index into the list of shape points for the start of the edge. |
| `end_shape_index` | Index into the list of shape points for the end of the edge. |
| `traversability` | TODO. |
| `use` | TODO. |
| `toll` | True if the edge has any toll. |
| `unpaved` | TODO. |
| `tunnel` | TODO. |
| `bridge` | TODO. |
| `roundabout` | TODO. |
| `internal_intersection` | TODO. |
| `drive_on_right` | TODO. |
| `surface` | TODO. |
| `sign` | TODO. |
| `travel_mode` | TODO. |
| `vehicle_type` | TODO. |
| `pedestrian_type` | TODO. |
| `bicycle_type` | TODO. |
| `transit_type` | TODO. |
| `id` | TODO. |
| `way_id` | TODO. |
| `weighted_grade` | TODO. |
| `max_upward_grade` | TODO. |
| `max_downward_grade` | TODO. |
| `lane_count` | TODO. |
| `cycle_lane` | TODO. |
| `bicycle_network` | TODO. |
| `sidewalk` | TODO. |
| `density` | TODO. |
| `speed_limit` | TODO. |
| `truck_speed` | TODO. |
| `truck_route` | TODO. |
| `end_node` | TODO. |

Each `sign` includes:

| Sign Item | Description |
| :--------- | :---------- |
| `exit_number` | TODO. |
| `exit_branch` | TODO. |
| `exit_toward` | TODO. |
| `exit_name` | TODO. |

Each `end_node` includes:

| Node Item | Description |
| :--------- | :---------- |
| `intersecting_edges` | TODO. |
| `elapsed_time` | TODO. |
| `admin_index` | TODO. |
| `type` | TODO. |
| `fork` | TODO. |
| `time_zone` | TODO. |

Each `intersecting_edge` includes:

| Intersecting Edge Item | Description |
| :--------- | :---------- |
| `begin_heading` | TODO. |
| `prev_name_consistency` | TODO. |
| `curr_name_consistency` | TODO. |
| `driveability` | TODO. |
| `cyclability` | TODO. |
| `walkability` | TODO. |

Each `admin` includes:

| Admin Item | Description |
| :--------- | :---------- |
| `country_code` | TODO. |
| `country_text` | TODO. |
| `state_code` | TODO. |
| `state_text` | TODO. |

