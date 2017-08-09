# Match GPS locations to mapped road segments

With [Mobility Explorer](https://mapzen.com/mobility/explorer), you can match GPS locations to roads and paths that have been mapped in OpenStreetMap. You can match, or snap, the GPS locations to the shape of the closest path, and view attribute values from that matched line. You can also generate turn-by-turn navigation instructions if you want to take your route again.

Here is an example of a line collected by GPS, shown with a dashed line, and the result when it is matched, shown with a solid line. Notice that the source GPS data shows some variation, compared to the straight line of the path to which it was matched.

![Original and matched lines](/images/mobility-explorer-source-matched-lines.png)

## Match a GPX file

To use map matching in Mobility Explorer, you need a [GPS Exchange Format file](https://en.wikipedia.org/wiki/GPS_Exchange_Format). This file, also known as GPX, is a common output format for recording a GPS track.

There are many apps designed to capture GPS locations from a mobile device and export them into GPX format. Mobility Explorer also includes sample GPX files you can use for experimenting.

Note that your GPX file is being processed locally in your browser and not actually being saved to your account. This means you will need to rerun the match if you want to see it again in the future. No changes are made to your source data.

1. Under `Try Map Matching`, click the drop-down list and choose either a sample GPX file or the option to use your own file.
2. To use your own file, click the `Upload` button and browse to the file on disk. Choose the transportation mode in which the GPX was recorded. Setting the mode helps the matching process find the correct path if there are overlapping or adjacent segments along a roadway.
3. On the map, you can view the raw line as it was collected in the GPX and see the start and end points. Click `View matched route` to attempt to align the GPX to the nearest line path. The map shows the matched line.
4. To see another trace, choose it from the list or upload your own.

## View the attributes of the matched route

When you have a GPX displaying on the map, you can style the line by either the grade or the speed and see different colors for the individual segments of the line.

The weighted grade provides an idea of the average grade for each segment, calculated from elevation data. It can be used to avoid hills when routing, particularly upward ones that require more energy. Within a segment, there may be some steeper slopes than this value indicates because a segment could go up and down over several hills. The `max upward grade` and `max downward grade` attributes provide more information about these.

The speed is from the speed limit value from the OpenStreetMap source data, if it exists, or a default speed value for that category of road otherwise. In both cases, keep in mind that these values are from the matched data, not from your source GPX.

![Matched line styled by segment grade](/images/mobility-explorer-style-by-grade.png)

When you click a segment on the map, you can see detailed attributes for the matched segment. Some attribute values, such as the name, pavement type, and road category, are from OpenStreetMap source data, while others are from Mapzen Mobility routing calculations.

1. Make sure you have a GPX file loaded and displaying on the map.
2. Style the line by the grade or speed limit along a segment.
3. Click a segment on the map to see a pop-up with the attribute being drawn.
4. In the table, view detailed attributes from OpenStreetMap about the line. These include the name, the OSM identification number, surface type (such as pavement), and the category of the road.

## Get turn-by-turn directions for the route

Perhaps you recently took a bicycle ride on a scenic trail and captured the path with a GPS. You can use map-matching to get turn-by-turn directions so you can repeat your route in the future or share it with others. You can copy the directions and paste the text into an email, for example.

![Matched directions for the route](/images/mobility-explorer-matched-directions.png)

1. Make sure you have a GPX file loaded and displaying on the map.
2. Click `Directions for the matched route`.

## Issues with matching

If you are attempting to match your own GPX file and get an error when you upload it, make sure your GPX file is valid and not corrupted.

If you can load the GPX but the map-matching process encountered an error or gave unexpected results, you see an entry for segments that could not be matched.

![Symbols for various states of matched lines](/images/mobility-explorer-match-segments-legend.png)

Sometimes, problems with matching are related to low GPS accuracy when you collected the trace. Reduced GPS accuracy may occur in urban environments, dense forests, valleys, or any other location where signals can be affected.

In addition, if a discontinuity exists in the OpenStreetMap source data, the results may be incomplete because there are no lines to match. Mobility Explorer has a list of unmatched segments, along with links to zoom to them on the map and see the areas in OpenStreetMap. If you notice incorrect or missing features in OpenStreetMap, fix them to improve the data quality for all users.

When the match results in an error, you can style the segments by attribute but are not able to get turn-by-turn directions for your route.

## Data credits

The images are from Mobility Explorer, which includes data from [Transitland](https://transit.land) and [OpenStreetMap](http://www.openstreetmap.org/).
