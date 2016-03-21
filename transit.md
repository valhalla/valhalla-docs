---
layout: page
category: blog
published: false
title: Let’s take a ~~road trip~~ transit journey to Valhalla, NY using Valhalla.
authors: [gknisely]
tags: [valhalla, routing, transitland]

---

![train.png](./images/train.png)

Back in February, the [Transitland team announced](https://mapzen.com/blog/help-us-catalog-the-transit-feeds-of-the-world) that they were aggregating transit data from around the world.  Now we are ready announce that we have completed a beta version of multi-modal routing utilizing [Transitland’s](https://transit.land/) data in San Francisco and NYC.

To clear up some items, as of right now, multimodal refers to routing via walking and public transit in [Valhalla](https://mapzen.com/projects/turn-by-turn).  In the future, we hope to add in motor cars and bicycles as additional modes.  However, for now, we can walk to our station, utilize our public transportation, disembark and walk to our final destination.  Of course, it is all based on time-based schedules of the public transportation.  By default, we begin our journey with the current date and time at the origin.  Also, some other defaults include the tendency to use rail over bus and we are prone to avoid transfers.  These values can all be tweaked to your liking.  Let’s try it out….

Let’s go from the Staten Island to Valhalla NY.  <i>I know, I know…absurdly long, but still fun to try out due to segments of this trip contain ferry, subway, and rail maneuvers.</i>

Ignoring the boring walking portions of the route.  Let’s look at the transit maneuvers.  Notice that we provide written depart and arrive time instructions for each transit portion of our trip.

<table>
<tr><td><img src="./images/transit-type-4.png" alt="ferry" style="width: 30px;"/>
</td><td>Enter the St. George Ferry Terminal station.</td><td>17 m</td></tr>
<tr><td><img src="./images/transit-type-4.png" alt="ferry" style="width: 30px;"/></td><td><i><font color = "6495ED">Depart: 9:30 PM from St. George Ferry Terminal.</font></i><br><br>Take the Staten Island Ferry. (1 stop)<br><br><i><font color = "6495ED">Arrive: 9:55 PM at Whitehall Ferry Terminal.</font></i></td><td>8.111 km</td></tr>
<tr><td><img src="./images/transit-type-4.png" alt="ferry" style="width: 30px;"/></td> <td>Exit the Whitehall Ferry Terminal station.</td><td>30 m</td></tr>
</table>

\~\~\~snip\~\~\~ goodbye walking maneuvers….  <img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/>

<table>
<tr><td><img src="./images/transit-type-1.png" alt="subway" style="width: 30px;"/></td><td>Enter the Bowling Green station.</td><td>22 m</td></tr>
<tr><td><img src="./images/transit-type-1.png" alt="subway" style="width: 30px;"/></td><td><i><font color = "6495ED">Depart: 10:04 PM from Bowling Green.</font></i><br><br>Take the 4 toward WOODLAWN. (8 stops)<br><br><i><font color = "6495ED">Arrive: 10:26 PM at 125 St.</font></i></td><td>12.86 km</td></tr>
<tr><td><img src="./images/transit-type-1.png" alt="subway" style="width: 30px;"/></td><td>Exit the 125 St station.</td><td>31 m</td></tr>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/></td><td>Head northwest on East 125th Street.</td><td>142 m</td></tr>
<tr><td><img src="./images/transit-type-2.png" alt="rail" style="width: 30px;"/></td><td>Enter the Harlem-125th St. station.</td><td>29 m</td></tr>
<tr><td><img src="./images/transit-type-2.png" alt="rail" style="width: 30px;"/></td><td><i><font color = "6495ED">Depart: 10:32 PM from Harlem-125th St.</font></i><br><br>Take the Harlem toward Southeast. (3 stops)<br><br><i><font color = "6495ED">Arrive: 11:03 PM at Valhalla.</font></i></td><td>33.316 km</td></tr>
<tr><td><img src="./images/transit-type-2.png" alt="rail"style="width: 30px;"/></td><td>Exit the Valhalla station.</td><td>28 m</td></tr>
</table>

## Use Options
As stated earlier, we can tweak our API defaults (use_bus, use_rail, and use_transfers).  The defaults are use_bus = 0.3, use_rail = 0.6, and use_transfers = 0.3.

- <p>**use_bus**  User's desire to use buses. Range of values from 0 (try to avoid buses) to 1 (strong preference for riding buses).
- <p>**use_rail**  User's desire to use rail/subway/metro. Range of values from 0 (try to avoid rail) to 1 (strong preference for riding rail).
- <p>**use_transfers**  User's desire to favor transfers Range of values from 0 (try to avoid transfers) to 1 (totally comfortable with transfers).

##Date and Time Options  <img src="./images/clock.png" alt="date and time options" style="width: 30px;"/>
For multimodal routes, we default to the current time; however, one can specify their depart date and time.  The date and time is in the local timezone to where the origin location resides.

- date_time
 - <p>**type**
   - 0 - Current depart time
   - 1 - Specified depart time
 - <p>**value**
   - The date and time to depart.  ISO 8601 format (YYYY-MM-DDThh:mm). For example "2016-03-29T08:06"

Let’s try it out.  Say you are in San Francisco @ Townsend St & 4th St and want to go to Beach St by Fishermans Wharf.  Also, you want to depart on March 29 @ 6:30 AM (2016-03-29T06:30) favoring buses.  Use_bus is now set at 0.5 and use_rail set at 0.3.

Notice in the results that we take the bus in the morning @ 6:35 AM and and avoid the rail lines.

<table>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/></td><td>Head southwest on Berry Street.</td><td>116 m</td></tr>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/></td><td>Turn right onto 4th Street/Fourth Street.</td><td>210 m</td></tr>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/></td><td>Turn right onto Townsend Street.</td><td>25 m</td></tr>
<tr><td><img src="./images/transit-type-3.png" alt="bus" style="width: 30px;"/></td><td><i><font color = "6495ED">Depart: 6:35 AM from TOWNSEND ST & 4TH ST.</font></i><br><br>Take the 30 toward the Marina District. (18 stops)<br><br><i><font color = "6495ED">Arrive: 6:56 AM at Columbus Ave & North Point St.</font></i></td><td>4.158 km</td></tr>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/></td><td>Head northwest on Columbus Avenue.</td><td>55 m</td></tr>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian" style="width: 30px;"/></td><td>Turn right onto Leavenworth Street.<br></td><td>184 m</td></tr>
<tr><td><img src="./images/mode-pedestrian.png" alt="pedestrian"style="width: 30px;"/></td><td>You have arrived at your destination.</td><td> </td></tr>
</table>

##Next Phase
Schedule tweaks and more and more schedule tweaks.  We know some of transit routes may not be the best; however, we need to start somewhere.  ;-)

The [Transitland](https://transit.land/) team will be implementing station hierarchies and egress points.  This will enable us to call out interstation transfers between different operators and lines and provide clean and accurate walking directions to station entrances.

We will also be adding more markets into [Valhalla](https://mapzen.com/projects/turn-by-turn); however, in the meantime try it out and have a look at our [documentation](https://mapzen.com/documentation/turn-by-turn/api-reference/) to see the structure of the API request and response.  As always, should you have any questions, write to us at [mobility@mapzen.com](mailto:mobility@mapzen.com).

< Embedded Demo link>