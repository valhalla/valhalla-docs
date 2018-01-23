Valhalla services are moving to Mapbox infrastructure effictive January 26 2018. Read further for steps on how to make the transition to avoid service interruptions.

## Valhalla to Mapbox transition guide

1. Sign up for Mapbox account [here](https://www.mapbox.com/signup/)
2. Get your Mapbox access token [here](https://www.mapbox.com/api-documentation/#access-tokens) and replace the Mapzen `access_key` parameter in your url with: `access_token=YOUR_TOKEN_HERE`
3. Adjust the URLs of your desired API(s) accordingly:

**Matrix**

    https://matrix.mapzen.com/sources_to_targets ->
        https://api.mapbox.com/valhalla/v1/sources_to_targets

**Route/Turn-by-Turn**

    https://valhalla.mapzen.com/route ->
        https://api.mapbox.com/valhalla/v1/route

**Map Matching**

    https://valhalla.mapzen.com/trace_route ->
        https://api.mapbox.com/valhalla/v1/trace_route
    https://valhalla.mapzen.com/trace_attributes ->
        https://api.mapbox.com/valhalla/v1/trace_attributes

**Elevation**

    https://elevation.mapzen.com/height ->
        https://api.mapbox.com/valhalla/v1/height

**Optimized route**

    https://matrix.mapzen.com/optimized_route ->
        https://api.mapbox.com/valhalla/v1/optimized_route

**Isochrone**

    https://matrix.mapzen.com/isochrone ->
        https://api.mapbox.com/valhalla/v1/isochrone
        
**Locate**

    https://valhalla.mapzen.com/locate ->
        https://api.mapbox.com/valhalla/v1/locate

## Having any issues? 
Refer to the Valhalla API [documentation](https://github.com/valhalla/valhalla-docs) or [contact Mapbox's support team](https://www.mapbox.com/contact/support/) for help!
