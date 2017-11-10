# Time-Distance Matrix service API reference

The Time-Distance Matrix service provides a quick computation of time and distance between a set of locations and returns them to you in the resulting matrix table.

For an interactive demo, see https://mapzen.com/products/time-distance-matrix/.

## Matrix service actions

You can request the following actions from the Time-Distance Matrix service: `/one_to_many?`, `/many_to_one?`, `/many_to_many?` or `/sources_to_targets?`. These queries compute different types of matrices: a row matrix for a `one_to_many`, a column matrix for a `many_to_one`, a square matrix for a `many_to_many` or any of the three matrices using `sources_to_targets`.  

The `/sources_to_targets` action takes a `sources` and `targets` parameter instead of the `locations` parameter. This allows you to set the origin locations separately from the destination locations. Because of this, the previously mentioned matrix types can be thought of as special cases of the `/sources_to_targets` request format. The difference here is that the set of origins may be disjoint (not overlapping) with the set of destinations. In other words, the target locations do not have to include any locations from source locations.

An action for `/weight?` is being considered for the future.

| Matrix type | Description |
| :--------- | :----------- |
| `one_to_many` | Returns a row vector of computed time and distance from the first (origin) location to each additional location provided. The first element in the row vector computed time and distance is [0,0.00]. |
| `many_to_one` | Returns a column vector of computed time and distance from each location to the last (destination) location provided. The last element in the row vector computed time and distance is [0,0.00]. |
| `many_to_many`| Returns a square matrix of computed time and distance from each location to every other location. The main diagonal of the square matrix is [0,0.00] all the way through.  |
| `sources_to_targets`| Can return a row, column or square matrix of computed time and distance, depending on your input for the sources and targets parameters.  |

## Inputs of the matrix service

An example request takes the form of `matrix.mapzen.com/one_to_many?json={}&api_key=`, where the `one_to_many?` represents the type of matrix and the JSON inputs inside the ``{}`` include an array of at least two locations and options for the route costing model.

For example, while at your office, you want to know the times and distances to walk to several restaurants where you could have dinner, as well as the times and distances from each restaurant to the train station for your commute home. This will help you determine where to eat. The API request for this uses `many_to_many` and `pedestrian` costing.

```
matrix.mapzen.com/many_to_many?json={"locations":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"costing":"pedestrian"}&id=ManyToMany_NYC_work_dinner&api_key=your-mapzen-api-key
```

These are similar examples as above, but using `sources_to_targets`.

`one-to-many using /sources_to_targets?`

```
matrix.mapzen.com/sources_to_targets?json={"sources":[{"lat":40.744014,"lon":-73.990508}],"targets":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"costing":"pedestrian"}&id=ManyToMany_NYC_work_dinner&api_key=your-mapzen-api-key
```

`many-to-one using /sources_to_targets?`

```
matrix.mapzen.com/sources_to_targets?json={"sources":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"targets":[{"lat":40.750552,"lon":-73.993519}],"costing":"pedestrian"}&id=ManyToMany_NYC_work_dinner&api_key=your-mapzen-api-key
```

`many-to-many using /sources_to_targets?`

```
matrix.mapzen.com/sources_to_targets?json={"sources":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"targets":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"costing":"pedestrian"}&id=ManyToMany_NYC_work_dinner&api_key=your-mapzen-api-key
```

There is an option to name your matrix request.  You can do this by appending the following to your request `&id=`.  The `id` is returned with the response so a user could match to the corresponding request.

The matrix service requires an API key. In a request, you must append your own API key to the URL, following `api_key=`. See the [Mapzen developer overview](https://mapzen.com/documentation/overview/) for more on API keys and rate limits.

### Location parameters

When using the `one_to_many`, `many_to_one` or `many_to_many` actions only, location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. External search services, such as [Mapzen Search](https://mapzen.com/products/search/geocoding) can be used to find places and geocode addresses, whose coordinates can be used as input to the Time-Distance Matrix service.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

You can refer to the [Turn-by-Turn location documentation](/turn-by-turn/api-reference.md#locations) for more information on specifying locations.  NOTE: Using `type` in addition to the `lat` and `lon` within the location parameter has no meaning for matrices.

### Source and target parameters

When using the `sources_to_targets` action, you specify sources and targets as ordered lists of one or more locations within a JSON array, depending on the type of matrix result you are expecting.

A source and target must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on.

| Source and target parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the source/target in degrees. |
| `lon` | Longitude of the source/target in degrees. |

### Costing parameters

The Time-Distance Matrix service uses the `auto`, `bicycle` and `pedestrian` costing models available in the Mapzen Turn-by-Turn service.  The **multimodal costing is not supported** for the Time-Distance Matrix service at this time.  Refer to the [Turn-by-Turn costing models](/turn-by-turn/api-reference.md#costing-models) and [costing options](/turn-by-turn/api-reference.md#costing-options) documentation for more on how to specify this input.

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
| `sources_to_targets` | Can return any of the 3 matrices of computed time and distance above, depending on your input for the sources and targets parameters. |
| `distance` | The computed distance between each set of points. Distance will always be 0.00 for the first element of the time-distance array for `one_to_many`, the last element in a `many_to_one`, and the first and last elements of a `many_to_many`. |
| `time` | The computed time between each set of points. Time will always be 0 for the first element of the time-distance array for `one_to_many`, the last element in a `many_to_one`, and the first and last elements of a `many_to_many`.  |
| `to_index` | The destination index into the locations array. |
| `from_index` | The origin index into the locations array. |
| `locations` | The specified array of lat/lngs from the input request.
| `units` | Distance units for output. Allowable unit types are mi (miles) and km (kilometers). If no unit type is specified, the units default to kilometers. |

See the [HTTP return codes](/turn-by-turn/api-reference.md#http-status-codes-and-conditions) for more on messages you might receive from the service.

## Sample matrix demonstration

If you want to see the results of the Time-Distance Matrix service, try this [interactive demo](https://mapzen.com/products/time-distance-matrix/).

The [code](https://github.com/valhalla/demos/tree/gh-pages/matrix) shows integration of the Time-Distance Matrix results into a map and a table. This demo does not use the `sources_to_targets` action call.
