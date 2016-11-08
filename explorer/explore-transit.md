# Explore transit data

Mobility Explorer starts with a default view of transit data.

## Explore transit routes

1. Search for a place in the box on the map or pan and zoom to find your area of interest.
2. Click `Show routes` to display routes on the map. The map updates to show transit route lines.
3. Hover over a line on the map to get a preview of more information about it.
4. Choose whether to display the lines by the type of transportation mode, such as bus or metro (subway), or the agency that operates the route.
5. To filter the map by a single route or operator, click it on the map or in the drop-down list.
6. Check `Show stops served by route` to view the transit stops along the route.
  The route line and stops that are displayed by default are a representation of the most common shape of that route. However, a route may be different at certain times, for example, in inbound or return directions, or to consider one-way roads. These differences are known as a RouteStopPattern in the Transitland API.
7. Click `Show route stop patterns` and a value to view the each unique combination of shape lines for trips and stops along this route.
  If you are also showing the stops, the stops are colored based on whether they are served by any stop along the route or only that particular RouteStopPattern.

## Explore transit stops and travel times to them

1. Search for a place in the box on the map or pan and zoom to find your area of interest.
2. Click `Show stops` to display transit stops on the map. The map updates to show points representing transit stops.
3. Hover over a point on the map to get a preview of information about it.
4. Click a point to get full details.
5. From the details panel, you can see which routes and operators serve that stop. Clicking the links for the individual routes or all routes serving the stop takes you to a query on the routes under `Show routes`. You can click your browser's Back button to return to your stops query.
  With a stop selected, you can view where you can travel from a point within a certain amount of time.
6. To view the distances you can travel from the stop location, click a mode of transportation. For example, click `walking` to see how far you can walk within intervals of 15 to 60 minutes. If you choose to travel by transit, you need to set the date and time of your departure.

## Explore operators

1. Search for a place in the box on the map or pan and zoom to find your area of interest.
2. Click `Show operators` to display the areas served by transit operators on the map. These polygons are created by connecting a line for the minimum bounding area of the extent of the routes and stops for that operator.
3. Hover over a polygon on the map to get a preview of more information about it.
4. To filter the map by a single operator, click it on the map or in the drop-down list. Because the polygons may overlap in areas with multiple operators, it may be easier to choose the operator by name from the list.
5. From the details panel, you can see which routes and stops are served by this operator. Viewing the routes and stops takes you to a query for `Show routes` or `Show stops`, respectively. You can click your browser's Back button to return to your operators query.
