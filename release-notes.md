
## Release Notes

Release Date: 2015-10-16

- **Through Location Types** - now supports locations with stop_type = through. Combines paths that meet at each through location type to create a single "leg" in a maneuver. Paths that continue at a through location will not create a U-turn unless the path enters a "dead-end" region (neighborhood with no outbound access).
- **Update shortcut edge logic** - Now skips long shortcut edges when close to the destination. This can lead to missing the proper connection if the shortcut is too long. Fixes #245 (thor).
- **Per mode service limits** - Update configuration to allow setting different maximum number of locations and distance per mode.
