The Travelling Salesperson service provides an answer to the following problem:  Given a list of locations, provide me with the most optimized route that reorders my intermediate locations, using my first location in the list as my starting point and last location as my ending point.

To use the service, you first need to obtain a developer API key from Mapzen. Sign in at https://mapzen.com/developers to create and manage your API keys. There are limitations on requests, maximum distances, and numbers of locations to prevent individual users from degrading the overall system performance.

You can request the following action from the Traveling Salesperson service: `/optimized?`. This query computes the most optimzed route from point 'a' to point 'n'.  Therefore, given a list of locations, an optimized route with stops at each intermediate location exactly one time, always starting at the first location in the list and ending at the last location.

Mapzen Travelling Salesperson is available for both commercial and non-commercial purposes. The [source code](https://github.com/valhalla) is open to view and modify, and contributions are welcomed.
