The Time-Distance Matrix service provides a quick computation of time and distance between a set of locations.

To use the service, you first need to obtain a developer API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys. There are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

You can request the following actions from the Time-Distance Matrix service: `/one_to_many?`, `/many_to_one?`, `/many_to_many?` or `/sources_to_targets?`. The first three queries compute different types of matrices: a row matrix for a `one_to_many`, a column matrix for a `many_to_one` or a square matrix for a `many_to_many`.  The `/sources_to_targets` can produce any of the three matrices.

Mapzen Time-Distance Matrix is available for both commercial and non-commercial purposes. The [source code](https://github.com/valhalla) is open to view and modify, and contributions are welcomed.
