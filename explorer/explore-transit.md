# Explore transit data

[Mobility Explorer](https://mapzen.com/mobility/explorer) highlights the connections between transit datasets, including among different transportation modes and operators. Search for a place or browse the map, and use the buttons to ask questions about fixed-route transit options in the area. For example, you can find a transit stop and see all the routes, available modes of transit, and transit agencies or operators that serve it. You can combine this information with Mapzen Mobility tools to see the area served by the stop to find out where you can travel from it within a certain amount of time.

Each time you click buttons or navigate the map on Mobility Explorer, you are sending a query to the [Transitland](https://transit.land/documentation/datastore/api-endpoints.html) or [Mapzen Mobility](https://mapzen.com/documentation/mobility/) APIs. You can click `See the Transitland API request for these results` to view the raw API request in your browser’s address bar and resulting JSON response from the server links. If you want to interact with these APIs programmatically, looking at the query can help you understand the components and create a properly formatted query that you can save and reuse in other projects that integrate these APIs.

Start exploring at https://mapzen.com/mobility/explorer by searching for a place or address in the box on the map or by panning and zooming to your area of interest. Each query or map update is maintained as a separate URL, which means that you can return to your previous map by clicking the back button in your browser window. You can also share this URL with others and they will see the same results that you see.

Hover over a point, line, or polygon on the map to get a preview of its name and other information, and click it for full details. As you move the map, you need to redo the search to get updated information.

## Explore transit routes

1. Click `Show routes` to display routes on the map. The map updates to show transit route lines.
2. To filter the map by a single route or operator, click it on the map or in the drop-down list.

  This is an example of all the routes operated by Metropolitan Transportation Authority in the New York City area.

  ![Routes by operator](/images/mobility-explorer-routes-by-operator.png)

3. Choose whether to display the lines by the type of transportation mode, such as bus or metro (subway), or the agency that operates the route.
4. Check `Show stops served by route` to view the transit stops along the route.
  The route line and stops that are displayed by default are a representation of the most common shape of that route. However, a route may be different at certain times, for example, in inbound or return directions, or to consider one-way roads. These differences are known as a [RouteStopPattern](https://transit.land/documentation/datastore/routes-and-route-stop-patterns.html) in the Transitland API.
5. Click `Show route stop patterns` and a value to view the unique combinations of shape lines for trips and stops along this route.
  If you are also showing the stops, the stops are colored based on whether they are served by any stop along the route or only that particular RouteStopPattern.

## Explore transit stops and travel times from them

1. Click `Show stops` to display transit stops on the map. The map updates to show points representing transit stops.

  Here is an example of transit stops in New York City.
  ![Transit stops within an area](/images/mobility-explorer-stops.png)

2. Click a point to get full details.
3. From the details panel, you can see which routes and operators serve that stop. Clicking the links for the individual routes or all routes serving the stop takes you to a query on the routes under `Show routes`.
  With a stop selected, you can view where you can travel from there within a certain amount of time.
4. To view the distances you can travel from the stop location, click a mode of transportation. For example, click `walking` to see how far you can walk within intervals of 15 to 60 minutes. If you choose to travel by transit, you need to set the date and time of your departure.

## Explore operators

1. Click `Show operators` to display the areas served by transit operators on the map. These polygons are created by connecting a line representing the minimum bounding area of the extent of the routes and stops for that operator.
2. To filter the map by a single operator, click it on the map or in the drop-down list. Because the polygons may overlap in areas with multiple operators, it may be easier to choose the operator by name from the list.
3. From the details panel, you can see which routes and stops are served by this operator. Viewing the routes and stops takes you to a query for `Show routes` or `Show stops`, respectively.

## Data credits

The images are from Mobility Explorer, which includes data from [Transitland](https://transit.land), [OpenStreetMap](http://www.openstreetmap.org/), and [CARTO](https://carto.com/).
