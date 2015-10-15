
# Release Notes

## Release Date: 2015-10-16

- **Through Location Types** - Improved support for locations with type = "through". Routes now combine paths that meet at each through location to create a single "leg" between locations with type = "break". Paths that continue at a through location will not create a U-turn unless the path enters a "dead-end" region (neighborhood with no outbound access).
- **Update shortcut edge logic** - Now skips long shortcut edges when close to the destination. This can lead to missing the proper connection if the shortcut is too long. Fixes #245 (thor).
- **Per mode service limits** - Update configuration to allow setting different maximum number of locations and distance per mode.
- **Fix shape index for trivial path** - Fix a bug where when building the the trip path for a "trivial" route (includes just one edge) where the shape index exceeded that size of the shape.
