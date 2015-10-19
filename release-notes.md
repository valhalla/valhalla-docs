
# Release Notes

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
