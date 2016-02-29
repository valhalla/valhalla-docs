# valhalla-docs
This repository contains most of the documentation for the Valhalla routing project. Deeper technical information about individual components of Valhalla can be found in the individual repositories contained with the Valhalla organization. This document gives some hints on where to look for specific information.

Documentation available within the valhalla-docs repository includes:

- [Introduction](./valhalla-intro.md) - This is the early history of Valhalla and the Mapzen Turn-by-Turn service based on it. Introduces the core team and describes overall objectives of the project and some insight on why we chose the name Valhalla.
- [Terminology](./terminology.md) - Contains commonly used terms and definitions within Valhalla. Also lists the various repositories.
- [Route API Reference](./api-reference.md) - The structure of API requests and responses to a Valhalla routing service is described here. This shows the JSON inputs and describes the JSON responses to form routes and directions.
-  [Matrix API Reference](./matrix/api-reference.md) - The structure of API requests and responses to a Valhalla time distance matrix service is described here. This shows the JSON inputs and describes the JSON responses to retrieve times and distances between locations.
-  [Elevation API Reference](./elevation/elevation-service.md) - The structure of API requests and responses to a Valhalla elevation service is described here. This shows the JSON inputs and describes the JSON responses to query elevation at specific locations.
- [Release Notes](./release-notes.md) - Contains information about changes  to the API, changes to the data import processing, new features, and general software updates of importance.
- [Add Routing to a Map](./add-routing-to-a-map.md) - A tutorial showing how to add Mapzen Turn-by-Turn (powered by Valhalla) to web based maps using the Leaflet Routing Machine with Mapzen plugins.
- [Decoding Shape](./decoding.md) - Describes how to decode the route path's shape (returned as an encoded polyline). Contains sample code in several languages.

Technical descriptions available in other Valhalla repositories includes:

- [Why Tiles?](../mjolnir/tree/master/docs/why_tiles.md) - Some of the objectives and reasons for designing a tiled, routing data set are included here.
- [OSM Connectivity Map](../mjolnir/tree/master/docs/connectivity.md) - Discusses creation of a "Connectivity Map" of OSM that uses Valhalla routing tiles to providesa first order of approximation of connectivity between locations.
- [Use of Administrative Data in Valhalla](../mjolnir/tree/master/docs/admins.md) - Discusses the importance of administrative information to routing and some of the ways that Valhalla uses adminstrative information.
- [Dynamic Costing](../sif/tree/master/docs/dynamic_costing.md) - Describes the basics of the dynamic, run-time path costing provided within the sif repository.
- [Elevation Influenced Bicycle Routing](../sif/tree/master/docs/elevation_costing.md) - Discusses how elevation is factored into bicycle costing to allow features such as "avoid hills".
