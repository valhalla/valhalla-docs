The Time-Distance Matrix service provides a quick computation of time and distance between a set of locations.

You can request the following actions from the Time-Distance Matrix service: `/one_to_many?`, `/many_to_one?`, `/many_to_many?` or `/sources_to_targets?`. The first three queries compute different types of matrices: a row matrix for a `one_to_many`, a column matrix for a `many_to_one` or a square matrix for a `many_to_many`.  The `/sources_to_targets` can produce any of the three matrices.

The [source code](https://github.com/valhalla) is open to view and modify, and contributions are welcomed.
