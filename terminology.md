# Common Mapzen Mobility terms

* break location - the start or end point of a route.
* cost - fixed costs in seconds that are added to both the path cost and the estimated time.
* costing model - set of costs for particular methods of travel, such as automobile or pedestrian.
* edge - a line connected between nodes
* factor - multiply the cost along an edge or road section in a way that influences the path to favor or avoid a particular attribute
* graph - a set of edges connected by nodes used for building a route
* location - a latitude, longitude coordinate pair, specified in decimal degrees that determines the routing and order of navigation.
* maneuver - an operation to be performed during navigation, such as a turn, and the expected duration of the movement.
* narration - textual guidance describing the maneuver to be performed, such as a turn, distance to travel, and expected time.
* path - the sequence of edges forming a route
* penalty - fixed costs in seconds that are only added to the path cost. Penalties can influence the route path determination but do not add to the estimated time along the path.
* route - sequence of edges and maneuvers forming the best travel path between locations given the available road network, costs, influence factors, and other inputs.
* short path - a route that attempts to minimize distance traveled over the constituent edges, but may not be the shortest distance.
* through location - an optional location to influence the route to travel through that location.
* tiled routing - method of building a path on graph data that has been split into square cells.
* time - the number of seconds estimated to complete a maneuver or trip, including any additional costs.
* trip - results of an entire route, including locations, legs, and maneuvers.
* height - with respect to elevation, the height above or below sea level at a specific location (lat,lng).
* height with range - computing the range (cumulative distance) and height for a series of lat,lng pairs of a line or shape.
