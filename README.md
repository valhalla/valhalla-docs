# valhalla-docs
This repository contains most of the documentation for the Valhalla routing project. Deeper technical information about individual components of Valhalla can be found in the individual repositories contained with the Valhalla organization. This document gives some hints on where to look for specific information.

Documentation available within the valhalla-docs repository includes:

- [Introduction](./valhalla-intro.md) - This is the early history of Valhalla. Introduces the core team and describes overall objectives of the project and some insight on why we chose the name Valhalla.
- [Terminology](./terminology.md) - Contains commonly used terms and definitions within Valhalla. Also lists the various repositories.
- [Route API Reference](./turn-by-turn/api-reference.md) - The structure of API requests and responses to a Valhalla routing service is described here. This shows the JSON inputs and describes the JSON responses to form routes and directions.
- [Map Matching API Reference](./map-matching/api-reference.md) - The structure of API requests and responses to a Valhalla map matching service is described here. This shows the JSON inputs and describes the JSON responses to perform map-matching. There are two flavors: 1) trace_route: froms a route result from the path that matches the input geometry, and 2) trace_attributes: returns detailed attribution along the path that matches the input geometry.
- [Locate API Reference](./locate/api-reference.md) - The structure of API requests and responses to a Valhalla locate service is described here. This shows the JSON inputs and describes the JSON responses to get detailed information about streets and interesections near a location.
- [Matrix API Reference](./matrix/api-reference.md) - The structure of API requests and responses to a Valhalla time distance matrix service is described here. This shows the JSON inputs and describes the JSON responses to retrieve times and distances between locations.
- [Optimized Route API Reference](./optimized/api-reference.md) - The structure of API requests and responses to a Valhalla optimized route service is described here. This shows the JSON inputs and describes the JSON responses to retrieve the route which optimizes the path through the input locations. This is essentially the Traveling Salesman Problem (TSP)
- [Elevation API Reference](./elevation/api-reference.md) - The structure of API requests and responses to a Valhalla elevation service is described here. This shows the JSON inputs and describes the JSON responses to query elevation at specific locations.
- [Release Notes](./release-notes.md) - Contains information about changes  to the API, changes to the data import processing, new features, and general software updates of importance.
- [Add Routing to a Map](./add-routing-to-a-map.md) - A tutorial showing how to add Valhalla routing to web based maps using the Leaflet Routing Machine with Valhalla plugins.
- [Decoding Shape](./decoding.md) - Describes how to decode the route path's shape (returned as an encoded polyline). Contains sample code in several languages.
- [Tile Description](./tiles.md) - Describes the tiling system used within Valhalla. Discusses the road hierarchy and tile numbering system.
- [Speed information](./speeds.md) - Describes the use of speed information and how OpenStreetMap tags impact speeds.

Data source listing and attribution information can be found here:

- [Data sources](../../../valhalla/blob/master/docs/mjolnir/data_sources.md) - A listing of data sources used within Valhalla routing tiles.
- [Attribution requirements](../../../valhalla/blob/master/docs/mjolnir/attribution.md).

Technical descriptions available in other Valhalla repositories includes:

- [Why Tiles?](../../../valhalla/blob/master/docs/mjolnir/why_tiles.md) - Some of the objectives and reasons for designing a tiled, routing data set are included here.
- [OSM Connectivity Map](../../../mjolnir/blob/master/docs/connectivity.md) - Discusses creation of a "Connectivity Map" of OSM that uses Valhalla routing tiles to provide a first order of approximation of connectivity between locations.
- [Use of Administrative Data in Valhalla](../../../valhalla/blob/master/docs/mjolnir/admins.md) - Discusses the importance of administrative information to routing and some of the ways that Valhalla uses adminstrative information.
- [Dynamic Costing](../../../valhalla/blob/master/docs/sif/dynamic-costing.md) - Describes the basics of the dynamic, run-time path costing provided within the sif repository.
- [Elevation Influenced Bicycle Routing](../../../valhalla/blob/master/docs/sif/elevation_costing.md) - Discusses how elevation is factored into bicycle costing to allow features such as "avoid hills".
