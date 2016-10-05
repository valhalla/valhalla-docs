## Release Date: 2016-TBD

* **Guidance Improvement**
  * Added the Hindi (hi-IN) narrative language

## Release Date: 2016-09-19

* **Guidance Improvement**
  * Added pirate narrative language
* **Routing Improvement**
  * Added the ability to include or exclude stops, routes, and operators in multimodal routing.
* **Service Improvement**
  * JSONify Error Response

## Release Date: 2016-08-30

* **Pedestrian Routing Improvement**
  * Fixes for trivial pedestrian routes

## Release Date: 2016-08-22

* **Guidance Improvements**
  * Added Spanish narrative
  * Updated the start and end edge heading calculation to be based on road class and edge use
* **Bicycle Routing Improvements**
  * Prevent getting off a higher class road for a small detour only to get back onto the road immediately. 
  * Redo the speed penalties and road class factors - they were doubly penalizing many roads with very high values. 
  * Simplify the computation of weighting factor for roads that do not have cycle lanes. Apply speed penalty to slightly reduce favoring
of non-separated bicycle lanes on high speed roads.
* **Routing Improvements**
  * Remove avoidance of U-turn for pedestrian routes. This improves use with map-matching since pedestrian routes can make U-turns.
  * Allow U-turns at dead-ends for driving (and bicycling) routes.
* **Service Additions**
  * Add support for multi-modal isochrones.
  * Added base code to allow reverse isochrones (path from anywhere to a single destination).
* **New Sources to Targets**
  * Added a new Matrix Service action that allows you to request any of the 3 types of time-distance matrices by calling 1 action.  This action takes a sources and targets parameter instead of the locations parameter.  Please see the updated Time-Distance Matrix Service API reference for more details.

## Release Date: 2016-08-08

 * **Service additions**
  * Latitude, longitude bounding boxes of the route and each leg have been added to the route results.
  * Added an initial isochrone capability. This includes methods to create an "isotile" - a 2-D gridded data set with time to reach each lat,lon grid from an origin location. This isoltile is then used to create contours at specified times. Interior contours are optionally removed and the remaining outer contours are generalized and converted to GeoJSON polygons. An initial version supporting multimodal route types has also been added.
 * **Data Producer Updates**
  * Fixed tranist scheduling issue where false schedules were getting added.
 * **Tools Additionas**
  * Added `valhalla_export_edges` tool to allow shape and names to be dumped from the routing tiles

## Release Date: 2016-07-19

 * **Guidance Improvements**
  * Added French narrative
  * Added capability to have narrative language aliases - For example: German `de-DE` has an alias of `de`
 * **Transit Stop Update** - Return latitude and longitude for each transit stop
 * **Data Producer Updates**
  * Added logic to use lanes:forward, lanes:backward, speed:forward, and speed:backward based on direction of the directed edge.
  * Added support for no_entry, no_exit, and no_turn restrictions.
  * Added logic to support country specific access. Based on country tables found here: http://wiki.openstreetmap.org/wiki/OSM_tags_for_routing/Access-Restrictions

## Release Date: 2016-06-08

 * **Bug Fix** - Fixed a bug where edge indexing created many small tiles where no edges actually intersected. This allowed impossible routes to be considered for path finding instead of rejecting them earlier.
 * **Guidance Improvements**
  * Fixed invalid u-turn direction
  * Updated to properly call out jughandle routes
  * Enhanced signless interchange maneuvers to help guide users
 * **Data Producer Updates**
  * Updated the speed assignment for ramp to be a percentage of the original road class speed assignment
  * Updated stop impact logic for turn channel onto ramp

## Release Date: 2016-05-19

 * **Bug Fix** - Fixed a bug where routes fail within small, disconnected "islands" due to the threshold logic in prior release. Also better logic for not-thru roads.

## Release Date: 2016-05-18

 * **Bidirectional A* Improvements** - Fixed an issue where if both origin and destination locations where on not-thru roads that meet at a common node the path ended up taking a long detour. Not all cases were fixed though - next release should fix. Trying to address the termination criteria for when the best connection point of the 2 paths is optimal. Turns out that the initial case where both opposing edges are settled is not guaranteed to be the least cost path. For now we are setting a threshold and extending the search while still tracking best connections. Fixed the opposing edge when a hierarchy transition occurs.
 * **Guidance Globalization** -  Fixed decimal distance to be locale based.
 * **Guidance Improvements**
  * Fixed roundabout spoke count issue by fixing the drive_on_right attribute.
  * Simplified narative by combining unnamed straight maneuvers
  * Added logic to confirm maneuver type assignment to avoid invalid guidance
  * Fixed turn maneuvers by improving logic for the following:
    * Internal intersection edges
    * 'T' intersections
    * Intersecting forward edges
 * **Data Producer Updates** - Fix the restrictions on a shortcut edge to be the same as the last directed edge of the shortcut (rather than the first one).

## Release Date: 2016-04-28

 * **Tile Format Updates** - Separated the transit graph from the "road only" graph into different tiles but retained their interconnectivity. Transit tiles are now hierarchy level 3.
 * **Tile Format Updates** - Reduced the size of graph edge shape data by 5% through the use of varint encoding (LEB128)
 * **Tile Format Updates** - Aligned `EdgeInfo` structures to proper byte boundaries so as to maintain compatibility for systems who don't support reading from unaligned addresses.
 * **Guidance Globalization** -  Added the it-IT(Italian) language file. Added support for CLDR plural rules. The cs-CZ(Czech), de-DE(German), and en-US(US English) language files have been updated.
 * **Travel mode based instructions** -  Updated the start, post ferry, and post transit insructions to be based on the travel mode, for example:
  * `Drive east on Main Street.`
  * `Walk northeast on Broadway.`
  * `Bike south on the cycleway.`

## Release Date: 2016-04-12

 * **Guidance Globalization** -  Added logic to use tagged language files that contain the guidance phrases. The initial versions of en-US, de-DE, and cs-CZ have been deployed.
 * **Updated ferry defaults** -  Bumped up use_ferry to 0.65 so that we don't penalize ferries as much.

## Release Date: 2016-03-31
 * **Data producer updates** - Do not generate shortcuts across a node which is a fork. This caused missing fork maneuvers on longer routes.  GetNames update ("Broadway fix").  Fixed an issue with looking up a name in the ref map and not the name map.  Also, removed duplicate names.  Private = false was unsetting destination only flags for parking aisles.         

## Release Date: 2016-03-30
 * **TripPathBuilder Bug Fix** - Fixed an exception that was being thrown when trying to read directed edges past the end of the list within a tile. This was due to errors in setting walkability and cyclability on upper hierarchies.

## Release Date: 2016-03-28

 * **Improved Graph Correlation** -  Correlating input to the routing graph is carried out via closest first traversal of the graph's, now indexed, geometry. This results in faster correlation and gaurantees the absolute closest edge is found.

## Release Date: 2016-03-16

 * **Transit type returned** -  The transit type (e.g. tram, metro, rail, bus, ferry, cable car, gondola, funicular) is now returned with each transit maneuver.
 * **Guidance language** -  If the language option is not supplied or is unsupported then the language will be set to the default (en-US). Also, the service will return the language in the trip results.
 * **Update multimodal path algorithm** - Applied some fixes to multimodal path algorithm. In particular fixed a bug where the wrong sortcost was added to the adjacency list. Also separated "in-station" transfer costs from transfers between stops.
 * **Data producer updates** - Do not combine shortcut edges at gates or toll booths. Fixes avoid toll issues on routes that included shortcut edges.

## Release Date: 2016-03-07

 * **Updated all APIs to honor the optional DNT (Do not track) http header** -  This will avoid logging locations.
 * **Reduce 'Merge maneuver' verbal alert instructions** -  Only create a verbal alert instruction for a 'Merge maneuver' if the previous maneuver is > 1.5 km.
 * **Updated transit defaults.  Tweaked transit costing logic to obtain better routes.** -  use_rail = 0.6, use_transfers = 0.3, transfer_cost = 15.0 and transfer_penalty = 300.0.  Updated the TransferCostFactor to use the transfer_factor correctly.  TransitionCost for pedestrian costing bumped up from 20.0f to 30.0f when predecessor edge is a transit connection.
 * **Initial Guidance Globalization** -  Partial framework for Guidance Globalization. Started reading some guidance phrases from en-US.json file.

## Release Date: 2016-02-22

 * **Use bidirectional A* for automobile routes** - Switch to bidirectional A* for all but bus routes and short routes (where origin and destination are less than 10km apart). This improves performance and has less failure cases for longer routes. Some data import adjustments were made (02-19) to fix some issues encountered with arterial and highway hierarchies. Also only use a maximum of 2 passes for bidirecdtional A* to reduce "long time to fail" cases.
 * **Added verbal multi-cue guidance** - This combines verbal instructions when 2 successive maneuvers occur in a short amount of time (e.g., Turn right onto MainStreet. Then Turn left onto 1st Avenue).

## Release Date: 2016-02-19

 * **Data producer updates** - Reduce stop impact when all edges are links (ramps or turn channels). Update opposing edge logic to reject edges that do no have proper access (forward access == reverse access on opposing edge and vice-versa). Update ReclassifyLinks for cases where a single edge (often a service road) intersects a ramp improperly causing the ramp to reclassified when it should not be. Updated maximum OSM node Id (now exceeds 4000000000). Move lua from conf repository into mjolnir.

## Release Date: 2016-02-01

 * **Data producer updates** - Reduce speed on unpaved/rough roads. Add statistics for hgv (truck) restrictions.

## Release Date: 2016-01-26

 * **Added capability to disable narrative production** - Added the `narrative` boolean option to allow users to disable narrative production. Locations, shape, length, and time are still returned. The narrative production is enabled by default. The possible values for the `narrative` option are: false and true
 * **Added capability to mark a request with an id** - The `id` is returned with the response so a user could match to the corresponding request.
 * **Added some logging enhancements, specifically [ANALYTICS] logging** - We want to focus more on what our data is telling us by logging specific stats in Logstash.

## Release Date: 2016-01-18

 * **Data producer updates** - Data importer configuration (lua) updates to fix a bug where buses were not allowed on restricted lanes.  Fixed surface issue (change the default surface to be "compacted" for footways).

## Release Date: 2016-01-04

 * **Fixed Wrong Costing Options Applied** - Fixed a bug in which a previous requests costing options would be used as defaults for all subsequent requests.

## Release Date: 2015-12-18

 * **Fix for bus access** - Data importer configuration (lua) updates to fix a bug where bus lanes were turning off access for other modes.
 * **Fix for extra emergency data** - Data importer configuration (lua) updates to fix a bug where we were saving hospitals in the data.
 * **Bicycle costing update** - Updated kTCSlight and kTCFavorable so that cycleways are favored by default vs roads.

## Release Date: 2015-12-17

 * **Graph Tile Data Structure update** - Updated structures within graph tiles to support transit efforts and truck routing. Removed TransitTrip, changed TransitRoute and TransitStop to indexes (rather than binary search). Added access restrictions (like height and weight restrictions) and the mode which they impact to reduce need to look-up.
 * **Data producer updates** - Updated graph tile structures and import processes.

## Release Date: 2015-11-23

 * **Fixed Open App for OSRM functionality** - Added OSRM functionality back to Loki to support Open App.

## Release Date: 2015-11-13

 * **Improved narrative for unnamed walkway, cycleway, and mountain bike trail** - A generic description will be used for the street name when a walkway, cycleway, or mountain bike trail maneuver is unnamed. For example, a turn right onto a unnamed walkway maneuver will now be: "Turn right onto walkway."
 * **Fix costing bug** - Fix a bug introduced in EdgeLabel refactor (impacted time distance matrix only).

## Release Date: 2015-11-3

 * **Enhance bi-directional A* logic** - Updates to bidirectional A* algorithm to fix the route completion logic to handle cases where a long "connection" edge could lead to a sub-optimal path. Add hierarchy and shortcut logic so we can test and use bidirectional A* for driving routes. Fix the destination logic to properly handle oneways as the destination edge. Also fix U-turn detection for reverse search when hierarchy transitions occur.
 * **Change "Go" to "Head" for some instructions** - Start, exit ferry.
 * **Update to roundabout instructions** - Call out roundabouts for edges marked as links (ramps, turn channels).
 * **Update bicycle costing** - Fix the road factor (for applying weights based on road classification) and lower turn cost values.

## Data Producer Release Date: 2015-11-2

 * **Updated logic to not create shortcut edges on roundabouts** - This fixes some roundabout exit counts.

## Release Date: 2015-10-20

 * **Bug Fix for Pedestrian and Bicycle Routes** - Fixed a bug with setting the destination in the bi-directional Astar algorithm. Locations that snapped to a dead-end node would have failed the route and caused a timeout while searching for a valid path. Also fixed the elapsed time computation on the reverse path of bi-directional algorithm.

## Release Date: 2015-10-16

 * **Through Location Types** - Improved support for locations with type = "through". Routes now combine paths that meet at each through location to create a single "leg" between locations with type = "break". Paths that continue at a through location will not create a U-turn unless the path enters a "dead-end" region (neighborhood with no outbound access).
 * **Update shortcut edge logic** - Now skips long shortcut edges when close to the destination. This can lead to missing the proper connection if the shortcut is too long. Fixes #245 (thor).
 * **Per mode service limits** - Update configuration to allow setting different maximum number of locations and distance per mode.
 * **Fix shape index for trivial path** - Fix a bug where when building the the trip path for a "trivial" route (includes just one edge) where the shape index exceeded that size of the shape.

## Release Date: 2015-09-28

 * **Elevation Influenced Bicycle Routing** - Enabled elevation influenced bicycle routing. A "use-hills" option was added to the bicycle costing profile that can tune routes to avoid hills based on grade and amount of elevation change.
 * **"Loop Edge" Fix** - Fixed a bug with edges that form a loop. Split them into 2 edges during data import.
 * **Additional information returned from 'locate' method** - Added information that can be useful when debugging routes and data. Adds information about nodes and edges at a location.
 * **Guidance/Narrative Updates** - Added side of street to destination narrative. Updated verbal instructions.
