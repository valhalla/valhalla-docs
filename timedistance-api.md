
# Valhalla Time Distance Service API Reference

The time distance service is a free, open-source web API (and c++ library) that provides a quick computation of time and distance between a set of locations. This page documents the inputs and outputs to the service.

The time distance service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in the [Thor GitHub repository](https://github.com/valhalla/thor/issues). You can also send a message to [routing@mapzen.com](mailto:routing@mapzen.com).
	
#### API keys and Service Limits

To use the Valhalla routing service, you must first obtain a free developer API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys.

Valhalla is a free, shared routing service. As such, there are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

The following time and distance limitations that are currently in place:

* `one_to_many` and `many_to_one` requests have a limit of <?> locations.
* `many_to_many` requests have a limit of <?> locations.
* max distance between any two locations is <will these be the same as the limits on the route api reference page?>.

Please also refer to the [Valhalla API Limits](https://mapzen.com/documentation/valhalla/api-reference/#api-keys-and-service-limits) to view the current routing limitations that are in place.

Limits may be increased in the future, but you can contact routing@mapzen.com if you encounter rate limit status messages and need higher limits in the meantime.

#### Request Actions/Methods

We currently provide one time distance action, '/timedistance?' that can be requested from the Time Distance Service.  This action can compute one of three types of matrices, either "matrix_type":"one_to_many", "matrix_type":"many_to_one" or "matrix_type":"many_to_many".  

In the future, we may also provide a second action, '/weight?'.

| Matrix Type | Description |
| :--------- | :----------- |
| `one_to_many` | To return a row vector of computed time and distance from the origin location to each additional location lat/lon provided. This means that the first element in the row vector computed time and distance will be [0,0.00]. |
| `many_to_one` | To return a row vector of computed time and distance from each of the location lat/lon's to the destination location provided. This means that the last element in the row vector computed time and distance will be [0,0.00]. |
| `many_to_many` | To return a square matrix of computed time and distance from each location lat/lon to every other location lat/lon.  This means that the first element and last element in the square matrix will be [0,0.00].  |


#### Input for the Time Distance API

An example request takes the form of `timedistance.mapzen.com/timedistance?json={}&api_key=`, where the JSON inputs inside the ``{}`` include an array of at least 2 locations, matrix type, name and options for the costing model, and output options.  <this may change to be the mapzen.com domain depending on outcome of design discussion>

Here is an example time distance request using pedestrian costing:

From the office, I want to know the times and distances to each restaraunt location for dinner, as well as the times and distances from each restaraunt to the train station for my journey home.  This will help me determine where I want to eat.

    valhalla.mapzen.com/timedistance?json={"locations":[{"lat":40.744014,"lon":-73.990508},{"lat":40.739735,"lon":-73.979713},{"lat":40.752522,"lon":-73.985015},{"lat":40.750117,"lon":-73.983704},{"lat":40.750552,"lon":-73.993519}],"matrix_type":"many_to_many","costing":"pedestrian","directions_options":{"units":"miles"}}&api_key=valhalla-xxxxxx
    
<this may change to its own timedistance domain depending on outcome of design discussion.  If so, would we want to use Matrix instead for domain name?>

Note that you must append your own [Valhalla API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.

#### Input Parameters

A location must include a latitude and longitude in decimal degrees. The coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. External search services, such as [Pelias](https://github.com/pelias) or [Nominatum](http://wiki.openstreetmap.org/wiki/Nominatim), can be used to find places and geocode addresses, whose coordinates can be used as input to the Time Distance service.

| Location parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Please also refer to the [Valhalla API Locations](https://mapzen.com/documentation/valhalla/api-reference/#locations) to view the full detail on Locations.

#### Costing

Please refer to the [Valhalla API Costing Models](https://mapzen.com/documentation/valhalla/api-reference/#costing-models) and [Costing Options](https://mapzen.com/documentation/valhalla/api-reference/#costing-options) to view the costing that is in place.

#### Output

The matrix results are returned with.

| Item | Description |
| :---- | :----------- |
| `matrix_type` | The specified type of matrix that was requested. Values may be either `one_to_many`, `many_to_one` or `many_to_many` |
| `one_to_many` | This will return a row vector (1 x n) of computed time and distance from the origin location to each additional location. |
| `many_to_one` | This will return a column vector (n X 1) of computed time and distance from each location provided to the destination location. |
| `many_to_many` | This will return a square matrix (n x n) of an array of computed time and distance from each location to every other location. |
| `location` | The specified lat/lon coordinates are returned from the input request. |
| `time` | The computed time between each set of points. Time will always be 0 for the first element of the time distance array for `one_to_many`, the last element in a `many_to_one`, and the first and last elements of a `many_to_many`.  |
| `distance` | The computed distance between each set of points. Distance will always be 0.00 for the first element of the time distance array for `one_to_many`, the last element in a `many_to_one`, and the first and last elements of a `many_to_many`. |

#### Return Codes and Conditions

Please refer to the [Valhalla API HTTP Return Codes](https://mapzen.com/documentation/valhalla/api-reference/#return-codes-and-conditions).
