
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

## Outputs of trace_route

TODO - link to turn-by-turn API doc

## Outputs of trace_attributes

