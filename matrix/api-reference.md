# Time-Distance Matrix service API reference

The Time-Distance Matrix service provides a quick computation of time and distance between a set of locations.

The Time-Distance matrix service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in the [Thor GitHub repository](https://github.com/valhalla/thor/issues) or send a message to [routing@mapzen.com](mailto:routing@mapzen.com).

## API keys and service limits

To use the Time-Distance Matrix service, you must first obtain an API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys.

As a shared service, there are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

The following limitations are currently in place.

* `max_locations` is  50 for `one_to_many`, `many_to_one`, and `many_to_many` requests.
* `max_distance` is the maximum "crow-flies" distance between two locations and is 200,000 meters (200 km) for all matrix types. For `one_to_many`, the distance between the first location and any of the others cannot exceed the maximum. For `many_to_one`, the distance between the last location and any of the others cannot exceed the maximum. Finally, for `many_to_many`, the distance between any pair of locations cannot exceed the maximum.
* rate limits are two queries per second and 5,000 queries per day.

You can also refer to the [Mapzen Turn-by-Turn documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#api-keys-and-service-limits) to view the current routing limitations that are in place for the Mapzen Turn-by-Turn service.

If you need more capacity, contact [routing@mapzen.com](mailto:routing@mapzen.com). You can also set up your own instance of Valhalla, which has access to functions similar to Mapzen's services.

## Matrix service actions

You can request the following actions from the Time-Distance Matrix service: `/one_to_many?`, `/many_to_one?` and `/many_to_many?`. These queries compute different types of matrices: a row matrix for a `one_to_many`, a column matrix for a `many_to_one` or a square matrix for a `many_to_many`.  

An action for `/weight?` is being considered for the future.

| Matrix type | Description |
| :--------- | :----------- |
| `one_to_many` | Returns a row vector of computed time and distance from the first (origin) location to each additional location provided. The first element in the row vector computed time and distance is [0,0.00]. |
| `many_to_one` | Returns a column vector of computed time and distance from each location to the last (destination) location provided. The last element in the row vector computed time and distance is [0,0.00]. |
| `many_to_many`| Returns a square matrix of computed time and distance from each location to every other location. The main diagonal of the square matrix is [0,0.00] all the way through.  |

## Inputs of the matrix service

An example request takes the form of `matrix.mapzen.com/one_to_many?json={}&api_key=`, where the `one_to_many?` represents the type of matrix and the JSON inputs inside the ``{}`` include an array of at least two locations and options for the route costing model.

For example, while at your office, you want to know the times and distances to walk to several restaurants where you could have dinner, as well as the times and distances from each restaurant to the train station for your commute home. This will help you determine where to eat. The API request for this uses `many_to_many` and `pedestrian` costing.

    matrix.mapzen.com/many_to_many?json={"locations":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"costing":"pedestrian"}&id=ManyToMany_NYC_work_dinner&api_key=matrix-xxxxxx

There is now an option to name your matrix request.  You can do this by appending the following to your request `&id=`.

Note that you must append your own [Matrix API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.

### Location parameters

A location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. External search services, such as [Mapzen Search](https://mapzen.com/documentation/search/) can be used to find places and geocode addresses, whose coordinates can be used as input to the Time-Distance Matrix service.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Refer to the [Turn-by-Turn location documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/#locations) for more information on specifying locations.

### Costing parameters

The Time-Distance Matrix service uses the costing models available in the Mapzen Turn-by-Turn service. Refer to the [Turn-by-Turn costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-models) and [costing options](https://mapzen.com/documentation/turn-by-turn/api-reference/#costing-options) documentation for more on how to specify this input.

### Other request options

| Options | Description |
| :------------------ | :----------- |
| `id` | Name your matrix request. If `id` is specified, the naming will be sent thru to the response. |

## Outputs of the matrix service

If a matrix request has been named using the optional `&id=` input, then the name will be returned as a string `id`.

These are the results of a request to the Time-Distance Matrix service.

| Item | Description |
| :---- | :----------- |
| `one_to_many` | Returns a row vector (1 x n) of computed time and distance from the first (origin) location to each additional location. |
| `many_to_one` | Returns a column vector (n X 1) of computed time and distance from each location provided to the last (destination) location. |
| `many_to_many` | Returns a square matrix (n x n) of an array of computed time and distance from each location to every other location. |
| `distance` | The computed distance between each set of points. Distance will always be 0.00 for the first element of the time-distance array for `one_to_many`, the last element in a `many_to_one`, and the first and last elements of a `many_to_many`. |
| `time` | The computed time between each set of points. Time will always be 0 for the first element of the time-distance array for `one_to_many`, the last element in a `many_to_one`, and the first and last elements of a `many_to_many`.  |
| `to_index` | The destination index into the locations array. |
| `from_index` | The origin index into the locations array. |
| `locations` | The specified array of lat/lngs from the input request.
| `units` | Distance units for output. Allowable unit types are mi (miles) and km (kilometers). If no unit type is specified, the units default to kilometers. |

See the [HTTP return codes](https://mapzen.com/documentation/turn-by-turn/api-reference/#return-codes-and-conditions) for more on messages you might receive from the service.

## Sample matrix demonstration

If you want to see the results of the Time-Distance Matrix service, try this [sample demonstration utility](http://valhalla.github.io/demos/matrix/) and [code](https://github.com/valhalla/demos/tree/gh-pages/matrix) that shows integration of the Time-Distance Matrix results into a map and a table.
