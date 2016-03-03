# Decode a route shape

Mapzen Turn-by-Turn uses the Google Maps encoded polyline format to store a series of latitude, longitude coordinates as a single string. Polyline encoding greatly reduces the size of the route response, especially for longer routes. A description is found here: [polyline encoding](https://developers.google.com/maps/documentation/utilities/polylinealgorithm).

Note that Mapzen Turn-by-Turn uses six digits of decimal precision, rather than five as referenced in the Google algorithms documentation.

Below are some sample algorithms to decode the string to create a list of latitude,longitude coordinates.

## JavaScript

Here is an example of decoding in JavaScript.

```
// This is adapted from the implementation in Project-OSRM
// https://github.com/DennisOSRM/Project-OSRM-Web/blob/master/WebContent/routing/OSRM.RoutingGeometry.js
polyline.decode = function(str, precision) {
    var index = 0,
        lat = 0,
        lng = 0,
        coordinates = [],
        shift = 0,
        result = 0,
        byte = null,
        latitude_change,
        longitude_change,
        factor = Math.pow(10, precision || 6);

    // Coordinates have variable length when encoded, so just keep
    // track of whether we've hit the end of the string. In each
    // loop iteration, a single coordinate is decoded.
    while (index < str.length) {

        // Reset shift, result, and byte
        byte = null;
        shift = 0;
        result = 0;

        do {
            byte = str.charCodeAt(index++) - 63;
            result |= (byte & 0x1f) << shift;
            shift += 5;
        } while (byte >= 0x20);

        latitude_change = ((result & 1) ? ~(result >> 1) : (result >> 1));

        shift = result = 0;

        do {
            byte = str.charCodeAt(index++) - 63;
            result |= (byte & 0x1f) << shift;
            shift += 5;
        } while (byte >= 0x20);

        longitude_change = ((result & 1) ? ~(result >> 1) : (result >> 1));

        lat += latitude_change;
        lng += longitude_change;

        coordinates.push([lat / factor, lng / factor]);
    }

    return coordinates;
};

```

## C++ 11

Here is an example of decoding in C++.

```
#include <vector>

constexpr double kPolylinePrecision = 1E6;
constexpr double kInvPolylinePrecision = 1.0 / kPolylinePrecision;

struct PointLL {
  float lat;
  float lon;
};

std::vector<PointLL> decode(const std::string& encoded) {
  size_t i = 0;     // what byte are we looking at

  // Handy lambda to turn a few bytes of an encoded string into an integer
  auto deserialize = [&encoded, &i](const int previous) {
    // Grab each 5 bits and mask it in where it belongs using the shift
    int byte, shift = 0, result = 0;
    do {
      byte = static_cast<int>(encoded[i++]) - 63;
      result |= (byte & 0x1f) << shift;
      shift += 5;
    } while (byte >= 0x20);
    // Undo the left shift from above or the bit flipping and add to previous
    // since its an offset
    return previous + (result & 1 ? ~(result >> 1) : (result >> 1));
  };

  // Iterate over all characters in the encoded string
  std::vector<PointLL> shape;
  int last_lon = 0, last_lat = 0;
  while (i < encoded.length()) {
    // Decode the coordinates, lat first for some reason
    int lat = deserialize(last_lat);
    int lon = deserialize(last_lon);

    // Shift the decimal point 5 places to the left
    shape.emplace_back(static_cast<float>(static_cast<double>(lat) *
                                          kInvPolylinePrecision),
                       static_cast<float>(static_cast<double>(lon) *
                                          kInvPolylinePrecision));

    // Remember the last one we encountered
    last_lon = lon;
    last_lat = lat;
  }
  return shape;
}
```
