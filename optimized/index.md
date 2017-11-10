The Optimized Route service provides an answer to the following problem:  Given a list of locations, provide me with the most optimized route that reorders my intermediate locations, using my first location in the list as my starting point and last location as my ending point.

You can request the following action from the Optimized Route service: `/optimized_route?`. This query computes the most optimized route from point `a` to point `n`.  Therefore, given a list of locations, an optimized route will stop at each destination location exactly one time, always starting at the first location in the list and ending at the last location.

The [source code](https://github.com/valhalla) is open to view and modify, and contributions are welcomed.
