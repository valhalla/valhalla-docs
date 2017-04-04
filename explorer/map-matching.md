# Match GPS locations to mapped road segments

With [Mobility Explorer](https://mapzen.com/mobility/explorer), you can match GPS locations to roads and paths that have been mapped in OpenStreetMap. You can match, or snap, the GPS locations to the shape of the closest path, and also obtain attribute values from that matched line.

To use map matching in Mobility Explorer, you need a [GPS Exchange Format file](https://en.wikipedia.org/wiki/GPS_Exchange_Format). This file, also known as GPX, is a common output format for recording a GPS track. There are many apps designed to capture GPS locations from a mobile device and export them into GPX format. Mobility Explorer also includes sample GPX files you can use to experiment with matching geometry and viewing attributes.

When you have a GPX displaying on the map, you can style the line by either the grade or the speed and see different colors for the individual segments of the line. The grade is calculated as a slope that considers the most extreme of the upward or downward value when both exist within a segment. The speed is from the speed limit value from the OpenStreetMap source data, if it exists, or a default speed value for that category of road otherwise. In both cases, keep in mind that these values are from the matched data, not from your source GPX.

When you click a segment on the map, you can see detailed attributes for the matched segment. Some attribute values, such as the name, pavement type, and road category, are from OpenStreetMap source data, while others are from Mapzen Mobility routing calculations.

If you are attempting to match your own GPX file and get an error when you upload it, make sure your GPX file is valid and not corrupted. If you can view the GPX on the map, but the map-matching alignment was not successful or gave unexpected results, please send a screenshot and your GPX file to routing@mapzen.com to help improve the results. Note that your file is being processed locally in your browser and not actually being saved to your account, so you will need to rerun the match if you want to see it again in the future.

Mobility Explorer demonstrates only a few of the capabilities of the Mapzen Map Matching service, and there are many other map-matching tasks you can do through the API. For example, perhaps you recently took a bicycle ride on a scenic trail and captured the path with a GPS. You can use the map-matching API to re-create the turn-by-turn navigation directions so you can repeat your route in the future or share it with others.

## Choose a GPX file to match

1. Under `Try Map Matching`, click the drop-down list and choose either a sample GPX file or the option to use your own file.
2. To use your own file, click the `Upload` button and browse to the file on disk. Choose the transportation mode in which the GPX was recorded. Setting the mode helps the matching process find the correct path if there are overlapping or adjacent segments along a roadway.
3. On the map, view the line as it was collected in the GPX and see the start and end points.
4. Click `Match this GPX` to attempt to align the GPX to the nearest line path. The map shows the matched line.

## View the attributes of the matched route

1. Make sure you have a GPX file loaded and displaying on the map.
2. Style the line by the grade or speed along a segment.
3. Click a segment on the map to see a pop-up with the attribute being drawn.
4. In the table, view detailed attributes from OpenStreetMap about the line. These include the name, the OSM identification number, surface type (such as pavement), and the category of the road.
5. You can click `See the Mapzen Mobility API request for these results` to view the raw API request in your browser’s address bar and resulting JSON response from the server.
6. When you are done, you can remove the match from the map and click `Try Map Match` to start over with a new GPX.
