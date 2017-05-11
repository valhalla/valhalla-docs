# Optimized Route service API reference

The Optimized Route service provides a quick computation of time and distance between a set of location sources and location targets and returns them in an optimized route order, along with the shape.

## Optimized route service action

You can request the following action from the Optimized Route service: `/optimized_route?`. Since an optimized route is really an extension of the `many_to_many` matrix, the first step is to compute a cost matrix by sending a `many_to_many` matrix request.  Then, we send our resulting cost matrix (resulting time or distance) to the optimizer which will return our optimized path.

| Optimized type | Description |
| :--------- | :----------- |
| `optimized_route` | Returns an optimized route stopping at each destination location exactly one time, always starting at the first location in the list and ending at the last location. This will result in a route with multiple legs.  |

## Inputs of the optimized route service

An example request takes the form of `matrix.mapzen.com/optimized_route?json={}&api_key=`, where the `optimized_route?` represents the type of query and the JSON inputs inside the ``{}`` include an array of at least four locations and options for the route costing model.

Here is an example of an Optimized Route scenario:

Given a list of cities and the distances and times between each pair, a salesperson wants to visit each city one time by taking the most optimized route and end at a destination (either return to origin or a different destination).

```
https://matrix.mapzen.com/optimized_route?json={"locations":[{"lat":40.042072,"lon":-76.306572},{"lat":39.992115,"lon":-76.781559},{"lat":39.984519,"lon":-76.6956},{"lat":39.996586,"lon":-76.769028},{"lat":39.984322,"lon":-76.706672}],"costing":"auto","directions_options":{"units":"miles"}}&api_key=your-mapzen-api-key
```

There is an option to name your optimized request. You can do this by appending the following to your request `&id=`.  The `id` is returned with the response so a user could match to the corresponding request.

The optimized route service requires an API key. In a request, you must append your own API key to the URL, following `api_key=`. See the [Mapzen developer overview](https://mapzen.com/documentation/overview/) for more on API keys and rate limits.

### Location parameters

A location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Refer to the [Turn-by-Turn location documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#locations) for more information on specifying locations.

### Costing parameters

The Optimized Route service uses the `auto`, `bicycle` and `pedestrian` costing models available in the Mapzen Turn-by-Turn service. The **multimodal costing is not supported** for the Optimized Route service at this time.  Refer to the [Turn-by-Turn costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-models) and [costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-options) documentation for more on how to specify this input.

### Other request options

| Options | Description |
| :------------------ | :----------- |
| `id` | Name your optimized request. If `id` is specified, the naming will be sent thru to the response. |

## Outputs of the optimized route service

If an optimized request has been named using the optional `&id=` input, then the name will be returned as a string `id`.

These are the results of a request to the Optimized Route service.

| Item | Description |
| :---- | :----------- |
| `optimized_route` | Returns an optimized route path from point 'a' to point 'n'.  Given a list of locations, an optimized route with stops at each intermediate location exactly one time, always starting at the first location in the list and ending at the last location.|
| `locations` | The specified array of lat/lngs from the input request.  The first and last locations in the array will remain the same as the input request.  The intermediate locations may be returned reordered in the response.  Due to the reordering of the intermediate locations, an `original_index` is also part of the `locations` object within the response.  This is an identifier of the location index that will allow a user to easily correlate input locations with output locations. |
| `units` | Distance units for output. Allowable unit types are mi (miles) and km (kilometers). If no unit type is specified, the units default to kilometers. |

## Error checking

The service checks the return from the `many_to_many` CostMatrix to see that all locations can be reached. If one or more cannot be reached, it returns an error and lists the location number that cannot be reached.  Currently, one location is listed at this time, even if more than one have an issue.

This is an example which it returns: `400::Location at index 3 is unreachable`

```
   https://matrix.mapzen.com/optimized_route?json={"locations":[{"lat":40.306600,"lon":-76.900022},{"lat":40.293246,"lon":-76.936230},{"lat":40.448678,"lon":-76.932885},{"lat":40.419753,"lon":-76.999632},{"lat":40.211050,"lon":-76.777071},{"lat":40.306600,"lon":-76.900022}],"costing":"auto"}&api_key=your-mapzen-api-key
```

See the [HTTP return codes](https://mapzen.com/documentation/turn-by-turn/api-reference/#return-codes-and-conditions) for more on messages you might receive from the service.

## Sample optimized route demonstration

If you want to see the results of the Optimized Route service, please try out the sample demonstration utility: [Optimized Route Test Utility](http://valhalla.github.io/demos/optimized_route/index.html#loc=13,40.748600,-73.969000)
