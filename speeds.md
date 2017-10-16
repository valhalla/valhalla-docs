# How are Road Speeds Assigned?

Valhalla does not currently support real-time or historical traffic information. We are working towards these capabilities and we feel that Valhalla's tiled data structures and dynamic costing approach will readily support traffic information when it becomes available.

### Assignment of Speed to Roadways

Valhalla routing data contains two attributes to denote speed. The most important for routing determination is named `speed`. Its units are kilometers per hour. The speed value along with the length of the roadway edge determine the time along the road section. A second speed value is `speed_limit`. This attribute contains the posted speed limit (if available) and can be used by mobile navigation applications to display the speed limit and possibly alert the driver when the speed limit is exceeded. An additional attribute, `speed_type`, defines whether the assigned routing speed is obtained from a speed limit or is based on the highway tag.

`speed` is assigned based on tags within the OpenStreetMap data as follows:

* `max_speed` - If a max_speed tag is available, that speed is used as the routing speed and the speed_limit is set to tht value.
* `highway` - If there is no max_speed tag then `speed` is set based on the highway tag. There are a default set of speeds for each highway tag. We plan to implement country specific default speeds based on highway tags as per: https://wiki.openstreetmap.org/wiki/OSM_tags_for_routing/Maxspeed.
* Road density - The road density (km of driveable roads / square kilometer) at each node in the routing graph is estimated during Valhalla data import. This road density is used to determine if a road is in a rural or urban area. Roads in urban areas have their`speed` reduced if there is no `max_speed` tag. This method may later be replaced by a more accurate measure of rural vs. urban region but the density method seems to produce adequate results for now.
