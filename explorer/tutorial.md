# Explore routes and stops

[Mobility Explorer](https://mapzen.com/mobility/explorer) highlights the connections between transit datasets, including among different transportation modes and operators. In this tutorial, you will ask questions about routes and stops and make your own transit map.

You can follow along with the location in the tutorial, or choose your own region.

To complete the tutorial, all you need is a browser and an Internet connection while you are working. No special knowledge of coding or transit data is needed. The tutorial should take about an hour to complete.

## Explore Transitland and Feed Registry

Transitland is an open-source database of transit information, and brings together many sources of transit data to build a directory of operators and feeds that can be edited by transit enthusiasts and developers. It has several components to it, including Feed Registry and Datastore.

- Feed Registry is a directory of transit operators and data feeds. Through the Feed Registry, you can browse operators, feeds, and usage and license information, as well as contribute additional feeds.
- Datastore brings together data from the Feed Registry with contributions, edits, and fixes from transit enthusiasts and developers. The Datastore provides a web API for querying and editing.

Mobility Explorer is a view into Datastore, allowing you to query the data using a user interface and see the results on a map.

## Meet Mobility Explorer

Now that you have seen the source data in Transitland, you can better understand where the data comes from that you see in Mobility Explorer.

You will first see which transit operators, or agencies, provide service in Oakland, California, and explore transit routes and stops in a later exercise.

1. In your browser, go to https://mapzen.com/mobility/explorer/. Mobility Explorer opens to a default location, with a map on the right and a sidebar on the left where you can visualize and analyze transit data.
2. In the search box on the map, type `Oakland, California`. The search box uses Mapzen Search, Mapzen's open-source geocoder, and the text automatically completes as you type. When you press Enter, the map extent updates and adds a pin in Oakland.
3. On the left, under `Visualize public transit networks`, click `Show Operators`. This shows polygons representing the area served by the transit service provided by each operator. Essentially, the polygon is created by drawing a line that connects at the endpoints of the transit routes for that operator.
4. Hover over the polygons on the map to see the operator name.

Sometimes, it can be hard to get information about a polygon because it is overlapped by other polygons. You can use the drop-down list of operators to see the service area.

5. Click the drop-down list of operators and click `Bay Area Rapid Transit (BART)`, which is a light-rail commuter train system, to see where this operator serves.

The sidebar shows details about the operator, including its name and website, and something called a Onestop ID. Under the operator details, there are also links to view the routes and stops for this operator.

A Onestop ID is a unique identifier from Transitland that helps label and connect data about public transit that are coming from many agencies. BART's Onestop ID is `o-9q9-bart`, with the first letter `o` indicating it is identifying an `operator`.

4. Zoom out so you can see the entire BART polygon by using the buttons on the map or your mouse wheel.
5. Click `View routes operated by Bay Area Rapid Transit`. Behind the scenes, this is querying the datastore for the transit route lines. In addition, when you do this, Mobility Explorer expands the `Show routes` section.
7. Explore BART's routes by hovering over a line on the map. If you click a route on the map or in the drop-down list, you can get details about the route.

## Get details about a route

When you are displaying routes in the map, you can choose whether to color them based on the mode, such as train or bus, or the operator.

_Tip: At anytime you want to start over with your search, click `Show routes` to search within the current map extent._

1. In the search box, type `2201 Broadway, Oakland` The search box has been customized to show only certain parts of matched addresses, so make sure you type the address and Oakland to make sure you are choosing the proper location.
2. Click `Show routes` to see the transit routes near this location. The total number of routes is shown on the drop-down list, and similar to BART, the individual route names are there, too. You can hover over a route on the map for more information.

Currently, the routes are being drawn in the same color, but you can style them so they are classified by the mode or the operator.
3. Under `Style routes by` click `Mode`. When you do this, each mode of transit (such as metro, bus, rail, or ferry) is drawn with a unique color. Depending on your zoom level, you may not see more than one mode available. If this happens, zoom out slightly and click `Redo search in map area`.
4. Under `Style routes by` click `Operator`. This shows each transit operator in a different color.
5. Hover over route `12` near the point, and click it on the map (or in the drop-down list) to see its details. This is a bus route operated by the Alameda-Contra Costa Transit District, and has a Onestop ID of `r-9q9p3-12`. The Onestop ID starts with the letter `r` to indicate it is a route data item.
6. Zoom out so you can see the full extent of this route, which goes between Oakland and the nearby city of Berkeley.
7. Check the box to `Show stops served by this route` to see dots on the map representing bus stops.

The route line and stops that are displayed by default are a representation of the most common shape of that route. However, a route may be different at certain times, for example, in inbound or return directions, or to consider one-way roads. These differences are known as a RouteStopPattern in the Transitland API.

8. Click the link to `Show route stop patterns` to view the unique combinations of shape lines for trips and stops along this route. For this route, there are two patterns, which you can click to see the differences between them.

## Extract GeoJSON for this query

Get GeoJSON, save it into a map.

## Explore transit stops near a location

1. Repeat the search for `2201 Broadway, Oakland` to return to your original location.
2. Click the stop immediately to the east of the marker, which is Broadway and W. Grand Avenue. This displays the details about the stop.

Similar to what you saw for the details of operators and routes, the stop also has a Onestop ID. This time, though, the Onestop ID is `s-9q9p1erf53-broadway~wgrandav`, where the `s` designates it is a transit stop.

You can see a list of the routes that serve this stop, which are different routes that are all from one transit operator. This is the power of Transitland, which aggregates data from many operators but connects them so they can be queried at the same time. For example, in downtown San Francisco, there are some locations where two different transit operators (BART and San Francisco Municipal Transportation Agency) have colocated underground metro rail stations, plus there are bus lines and light rail stops on the surface. You can click that stop and get information for all the transit options there, even if they are served by different operators.

## View travel times from a location

With a point on the map, either a selected stop or marker from a search, you can see where you can travel within a certain amount of time. You can specify the mode of transportation, such as walking, biking, driving, or taking transit, and see a series of rings to represent where you can reach within various increments of time, ranging from 15 to 60 minutes.

This is known as an isochrone, which is a line that connects points of equal travel time about a given location, from the Greek roots of `iso` for equal and `chrone` for time. Isochrone functionality is also sometimes referred to as a service area, a drive-time analysis to show where you can drive from a point within a certain time, or a walkshed. A walkshed, which is a transportation planning term, calculates an area within a range of a location that can be reached by walking (or a bikeshed for areas that can be traveled by bicycle within those time ranges).

The analysis comes from the [Mapzen Isochrone](https://mapzen.com/documentation/mobility/isochrone/api-reference/) service, which you can use as an API in your own apps.

1. Repeat the search for `2201 Broadway, Oakland` to return to your original location.
2. Under `Analyze access`, click `Generate isochrones`.
3. Click the option for `walking` to see where you can walk from this point. The map updates to show color-coded rings that represent where you can reach within 15-, 30-, 45-, and 60-minute time increments.
4. Try the biking and driving options to see how the shapes compare to walking. _Note: The driving map does not yet consider current traffic conditions._
5. Click `transit` to view where you can travel by transit. By default, isochrones are calculated for the current time.
6. Click the clock button and try changing the departure time to be during workday commute hours, weekends, and after midnight to see the differences. Typically, transit service is reduced at night and on weekends, so it is likely that the polygons are much smaller then than during working hours.
