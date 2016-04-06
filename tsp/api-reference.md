# Travelling Salesperson service API reference

The Travelling Salesperson service provides a quick computation of time and distance between a set of locations.

The Travelling Salesperson service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in the [Thor GitHub repository](https://github.com/valhalla/thor/issues) or send a message to [routing@mapzen.com](mailto:routing@mapzen.com).

## API keys and service limits

To use the Travelling Salesperson service, you must first obtain an API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys.

As a shared service, there are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

The following limitations are currently in place.

* `max_locations` is  50 for `optimized` requests.
* `max_distance` is the maximum "crow-flies" distance between two locations and is 200,000 meters (200 km) for optimized routes. This is identical to `many_to_many` matrix, where the distance between any pair of locations cannot exceed the maximum.
* rate limits are two queries per second and 5,000 queries per day.

You can also refer to the [Mapzen Turn-by-Turn documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#api-keys-and-service-limits) to view the current routing limitations that are in place for the Mapzen Turn-by-Turn service.

If you need more capacity, contact [routing@mapzen.com](mailto:routing@mapzen.com). You can also set up your own instance of Valhalla, which has access to functions similar to Mapzen's services.

## Optimized service action

You can request the following action from the Travelling Salesperson service: `/optimized?`. Since an optimized route is really an extension of the `many_to_many` matrix, the first step is to compute a time-distance matrix by sending a `many_to_many` matrix request.  Then, we send our resulting cost matrix (resulting time or distance) to the optimizer which will return our optimized path. 

| Optimized type | Description |
| :--------- | :----------- |
| `optimized` | Returns an optimized route stopping at each intermediate location exactly one time, always starting at the first location in the list and ending at the last location. This will result in a route with multiple legs.  |

## Inputs of the travelling salesperson service

An example request takes the form of `matrix.mapzen.com/optimized?json={}&api_key=`, where the `optimized?` represents the type of query and the JSON inputs inside the ``{}`` include an array of at least 4 locations and options for the route costing model.

Here is an example of a Travelling Salesperson scenario:

Given a list of cities and the distances/times between each pair, a salesperson wants to visit each city one time taking the most optimized route and end at his/her destination (either return to origin or a different destination). 

    matrix.mapzen.com/optimized?json={"locations":[{"lat":40.042072,"lon":-76.306572},{"lat":39.992115,"lon":-76.781559},{"lat":39.984519,"lon":-76.6956},{"lat":39.996586,"lon":-76.769028},{"lat":39.984322,"lon":-76.706672}],"costing":"auto","units":"mi"}&api_key=matrix-xxxxxx

There is an option to name your optimized request.  You can do this by appending the following to your request `&id=`.  The `id` is returned with the response so a user could match to the corresponding request.

Note that you must append your own [Matrix API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.

### Location parameters

A location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. External search services, such as [Mapzen Search](https://mapzen.com/documentation/search/) can be used to find places and geocode addresses, whose coordinates can be used as input to the Travelling Salesperson service.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Refer to the [Turn-by-Turn location documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#locations) for more information on specifying locations.

### Costing parameters

The Travelling Salesperson service uses the costing models available in the Mapzen Turn-by-Turn service. Refer to the [Turn-by-Turn costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-models) and [costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-options) documentation for more on how to specify this input.

### Other request options

| Options | Description |
| :------------------ | :----------- |
| `id` | Name your optimized request. If `id` is specified, the naming will be sent thru to the response. |

## Outputs of the travelling salesperson service

If an optimized request has been named using the optional `&id=` input, then the name will be returned as a string `id`.

These are the results of a request to the Travelling Salesperson service.

| Item | Description |
| :---- | :----------- |
| `optimized` | Returns an optimized route path from point 'a' to point 'n'.  Given a list of locations, an optimized route with stops at each intermediate location exactly one time, always starting at the first location in the list and ending at the last location.|
| `locations` | The specified array of lat/lngs from the input request.  The first and last locations in the array will remain the same as the input request.  The intermediate locations may be returned reordered in the response.
| `units` | Distance units for output. Allowable unit types are mi (miles) and km (kilometers). If no unit type is specified, the units default to kilometers. |

See the [HTTP return codes](https://mapzen.com/documentation/turn-by-turn/api-reference/#return-codes-and-conditions) for more on messages you might receive from the service.

## Sample travelling salesperson demonstration

If you want to see the results of the Travelling Salesperson service, a sample demonstration utility is COMING SOON.
