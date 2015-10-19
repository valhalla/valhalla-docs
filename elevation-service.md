#Elevation service API reference

Mapzen's elevation service is a free, open-source web API (and C++ library) that provides digital elevation model (DEM) data as the result of a query. The elevation service data has many applications when combined with other routing and naivgation data, including computing the steepness of edges or generating an elevation profile along a route. This page documents the inputs and outputs to the service.

The elevation service is in active development. You can follow the [Mapzen blog](https://mapzen.com/blog) to get updates. To report software issues or suggest enhancements, open an issue in [Skadi GitHub repository](https://github.com/valhalla/skadi/issues). If you find a unique use for the elevation service, let the developers know at [routing@mapzen.com](mailto:routing@mapzen.com)! 

##API keys and service limits

To use Mapzen's elevation service, you must first obtain a free, Elevation Service developer API key. Sign in at https://mapzen.com/developers to create and manage your API keys.

This is a free, shared service. As such, there are limitations on the number of sampling points to prevent individual users from degrading the overall system performance. The limits are related to the number of points for which you request elevations, rather than the resolution of the DEM in that area. 

Limits may be increased in the future, but you can contact routing@mapzen.com if you encounter rate limit status messages and need higher limits in the meantime.

##Inputs of the elevation service

The elevation service currently has a single action, `/height?`, that can be requested. The `height` provides the elevation at a set of input locations, which are specified as either a `shape` or an `encoded_polyline`. The shape option uses an ordered list of two or more locations within a JSON array, while an [encoded polyline](https://developers.google.com/maps/documentation/utilities/polylinealgorithm?hl=en) stores multiple locations within a single string. If you include a `range` parameter and set it to `true`, both the height and cumulative distance are returned for each point. 

An elevation service request takes the form of `elevation.mapzen.com/height?json={}&api_key=`, where the JSON inputs inside the ``{}`` includes location information and the optional range parameter. Note that you must append your own [API key](https://mapzen.com/developers) to the URL, following `&api_key=` at the end.

###Use a shape list for input locations

A `shape` request must include a latitude and longitude in decimal degrees, and the locations are visited in the order specified. The input coordinates can come from many input sources, such as a GPS location, a point or a click on a map, a geocoding service, and so on. 

These parameters are available for `shape`.

| Shape parameters | Description |
| :--------- | :----------- |
| `lat` | Latitude of the location in degrees. |
| `lon` | Longitude of the location in degrees. |

Here is an example of a profile request using `shape`:

    elevation.mapzen.com/height?json={"range":true,"shape":[{"lat":40.712431,"lon":-76.504916},{"lat":40.712275,"lon":-76.605259},{"lat":40.712122,"lon":-76.805694},{"lat":40.722431,"lon":-76.884916},{"lat":40.812275,"lon":-76.905259},{"lat":40.912122,"lon":-76.965694}]}&api_key=elevation-xxxxxx

This request provides `shape` points near Pottsville, Pennsylvania. The resulting profile response displays the input shape, as well as the `range` and `height` (as `range_height` in the response) for each point.

    {"shape":[{"lat":40.712433,"lon":-76.504913},{"lat":40.712276,"lon":-76.605263},{"lat":40.712124,"lon":-76.805695},{"lat":40.722431,"lon":-76.884918},{"lat":40.812275,"lon":-76.905258},{"lat":40.912121,"lon":-76.965691}],"range_height":[[0,307],[8467,272],[25380,204],[32162,204],[42309,180],[54533,198]]}
    
Without the `range`, the result looks something like this, with only a `height`:

    {"shape":[{"lat":40.712433,"lon":-76.504913},{"lat":40.712276,"lon":-76.605263},{"lat":40.712124,"lon":-76.805695},{"lat":40.722431,"lon":-76.884918},{"lat":40.812275,"lon":-76.905258},{"lat":40.912121,"lon":-76.965691}],"height":[307,272,204,204,180,198]}

###Use an encoded polyline for input locations

The `encoded_polyline` parameter is a string of a polyline-encoded shape and has the following parameters.

| Encoded polyline parameters | Description |
| :--------- | :----------- |
| `encoded_polyline` | A set of encoded latitude, longitude pairs of a line or shape.|

Here is an example `encoded_polyline` request:

    elevation.mapzen.com/height?json={"range":true,"encoded_polyline":"s{cplAfiz{pCa]xBxBx`AhC|gApBrz@{[hBsZhB_c@rFodDbRaG\\ypAfDec@l@mrBnHg|@?}TzAia@dFw^xKqWhNe^hWegBfvAcGpG{dAdy@_`CpoBqGfC_SnI{KrFgx@?ofA_Tus@c[qfAgw@s_Agc@}^}JcF{@_Dz@eFfEsArEs@pHm@pg@wDpkEx\\vjT}Djj@eUppAeKzj@eZpuE_IxaIcF~|@cBngJiMjj@_I`HwXlJuO^kKj@gJkAeaBy`AgNoHwDkAeELwD|@uDfC_i@bq@mOjUaCvDqBrEcAbGWbG|@jVd@rPkAbGsAfDqBvCaIrFsP~RoNjWajBlnD{OtZoNfXyBtE{B~HyAtEsFhL_DvDsGrF_I`HwDpGoH|T_IzLaMzKuOrFqfAbPwCl@_h@fN}OnI"}&api_key=elevation-xxxxxx

###Get height and distance with the range parameter

The `range` parameter is a boolean value that controls whether or not the returned array is one-dimensional (height only) or two-dimensional (with a range and height). This can be used to generate a graph along a route, because a 2D-array has values for x (the range) and y (the height) at each shape point. Steepness or gradient can also be computed from a profile request. 

The `range` is optional and assumed to be `false` if omitted.

| Range parameters | Description |
| :--------- | :----------- |
| `range` | `true` or `false`. Defaults to `false`.|

##Outputs of the elevation service

The profile results are returned with the form of shape (shape points or encoded polylines) that was supplied in the request, along with a 2D array representing the x and y of each input point in the elevation profile.

| Item | Description |
| :---- | :----------- |
| `shape` | The specified shape coordinates from the input request. |
| `encoded_polyline` | The specified encoded polyline coordinates from the input request. |
| `range_height` | The 2D array of range (x) and height (y) per input latitude, longitude coordinate. |
| `x coordinate` | The range or distance along the input locations. It is the cumulative distance along the previous latitiude, longitude coordinates up to the current coordinate. The x-value for the first coordinate in the shape will always be 0. |
| `y coordinate` | The height or elevation of the associated latitude, longitude pair. The height is returned as `null` if no height data exists for a given location. |
| `height` | An array of height for the associated latitude, longitude coordinates. |

##Data sources and known issues

Currently, the underlying data sources for the service are a mix of [SRTM](http://www2.jpl.nasa.gov/srtm/), [GMTED](http://topotools.cr.usgs.gov/gmted_viewer/) and [GEBCO](http://www.gebco.net/data_and_products/gridded_bathymetry_data/) DEMs. These sets provide global coverage at varying resolutions up to approximately 30 meters. It should be noted that both SRTM and GMTED fill oceans and other bodies of water with a value of zero to indicate mean sea level; in these areas, GEBCO provides bathymetry (as well as in regions which are not coverged by SRTM and GMTED). Many other classical DEM-related issues occur in these datasets. It is not uncommon to see large variations in elevation in areas with large buildings and other such structures. We are considering how to best integrate NED and NRCAN sources, and are always looking for better datasets. If you find any data issues or can suggest any supplemental open datasets, please let us know by filing an issue in [valhalla/skadi](https://github.com/valhalla/skadi).
