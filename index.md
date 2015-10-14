[Mapzen Turn-by-Turn](https://mapzen.com/projects/valhalla) is a free, open-source routing service that lets you integrate routing and navigation into a web or mobile application. The service works globally, and provides dynamic and customizable routing by driving, walking, or bicycling, with clear directions for maneuvers along the route.

To use the service, you must first obtain a developer API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys. There are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

To build a route, you specify the locations to visit and any options within a JSON array. A location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service such as [Mapzen Search](https://mapzen.com/projects/search), and so on. 



Mapzen Turn-by-Turn is powered by the Valhalla routing engine, and is available for both commercial and non-commercial purposes. The [source code](https://github.com/valhalla) is open to view and modify, and contributions are welcomed.


- a route line between map locations (also known as waypoints)
- a text narrative of maneuvers to perform on the route
- distances along your route and estimated travel times
- functionality to drag the route start and endpoints to get a different path
- the ability change the mode of transportation, such as automobile or pedestrian



