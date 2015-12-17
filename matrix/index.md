The time-distance matrix service is an open-source web API (and c++ library) that provides a quick computation of time and distance between a set of locations. Time-distance matrix computations can be performed for any costing method or travel mode available (auto, bicycle or pedestrian).

To use the matrix, you must first obtain a developer API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys.

An example request takes the form of 

`matrix.mapzen.com/one_to_many?json={}&api_key=`

where the JSON inputs inside the {} include an array of at least 2 locations and options for the costing model. A location must include a latitude and longitude in decimal degrees. External search services such as Mapzen Search can be used to find places and geocode addresses, whose coordinates can be used as input to the Time Distance service.

There are currently three time-distance matrix actions, `/one_to_many?`, which returns a row matrix; `/many_to_one?`, which returns a column matrix; and `/many_to_many?`, which returns a square matrix. The service can return distance in either kilometers or miles.
