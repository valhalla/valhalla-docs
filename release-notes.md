
# Release Notes

## Release Date: 2016-01-04

- **Fixed Wrong Costing Options Applied** - Fixed a bug in which a previous requests costing options would be used as defaults for all subsequent requests.

## Release Date: 2015-12-18

- **Fix for bus access** - Data importer configuration (lua) updates to fix a bug where bus lanes were turning off access for other modes.

## Release Date: 2015-12-17

- **Graph Tile Data Structure update** - Updated structures within graph tiles to support transit efforts and truck routing. Removed TransitTrip, changed TransitRoute and TransitStop to indexes (rather than binary search). Added access restrictions (like height and weight restrictions) and the mode which they impact to reduce need to look-up.
- **Data producer updates** - Updated graph tile structures and import processes.

## Release Date: 2015-11-23

- **Fixed Open App for OSRM functionality** - Added OSRM functionality back to Loki to support Open App.

## Release Date: 2015-11-13

- **Improved narrative for unnamed walkway, cycleway, and mountain bike trail** - A generic description will be used for the street name when a walkway, cycleway, or mountain bike trail maneuver is unnamed. For example, a turn right onto a unnamed walkway maneuver will now be: "Turn right onto walkway."
- **Fix costing bug** - Fix a bug introduced in EdgeLabel refactor (impacted time distance matrix only).

## Release Date: 2015-11-3

- **Enhance bi-directional A* logic** - Updates to bidirectional A* algorithm to fix the route completion logic to handle cases where a long "connection" edge could lead to a sub-optimal path. Add hierarchy and shortcut logic so we can test and use bidirectional A* for driving routes. Fix the destination logic to properly handle oneways as the destination edge. Also fix U-turn detection for reverse search when hierarchy transitions occur.
- **Change "Go" to "Head" for some instructions** - Start, exit ferry.
- **Update to roundabout instructions** - Call out roundabouts for edges marked as links (ramps, turn channels).
- **Update bicycle costing** - Fix the road factor (for applying weights based on road classification) and lower turn cost values.

## Data Producer Release Date: 2015-11-2

- **Updated logic to not create shortcut edges on roundabouts** - This fixes some roundabout exit counts.

## Release Date: 2015-10-20

- **Bug Fix for Pedestrian and Bicycle Routes** - Fixed a bug with setting the destination in the bi-directional Astar algorithm. Locations that snapped to a dead-end node would have failed the route and caused a timeout while searching for a valid path. Also fixed the elapsed time computation on the reverse path of bi-directional algorithm.

## Release Date: 2015-10-16

- **Through Location Types** - Improved support for locations with type = "through". Routes now combine paths that meet at each through location to create a single "leg" between locations with type = "break". Paths that continue at a through location will not create a U-turn unless the path enters a "dead-end" region (neighborhood with no outbound access).
- **Update shortcut edge logic** - Now skips long shortcut edges when close to the destination. This can lead to missing the proper connection if the shortcut is too long. Fixes #245 (thor).
- **Per mode service limits** - Update configuration to allow setting different maximum number of locations and distance per mode.
- **Fix shape index for trivial path** - Fix a bug where when building the the trip path for a "trivial" route (includes just one edge) where the shape index exceeded that size of the shape.

## Release Date: 2015-09-28

- ** Elevation Influenced Bicycle Routing** - Enabled elevation influenced bicycle routing. A "use-hills" option was added to the bicycle costing profile that can tune routes to avoid hills based on grade and amount of elevation change.
- **"Loop Edge" Fix** - Fixed a bug with edges that form a loop. Split them into 2 edges during data import.
- **Additional information returned from 'locate' method** - Added information that can be useful when debugging routes and data. Adds information about nodes and edges at a location.
- **Guidance/Narrative Updates** - Added side of street to destination narrative. Updated verbal instructions.
