* /route?
  * directions_options
    * units
      * miles
      * kilometers
  * date_time
    * type
    * value
  * locale
    * en-US
    * de-DE
    * cz-CS
    * es-ES
    * fr-FR
  * costing
    * auto
    * pedestrian
    * multimodal
    * bike
    * bus
    * auto_shorter
  * costing_options
    * auto
      * maneuver_penalty
      * destination_only_penalty
      * alley_penalty
      * gate_cost
      * toll_booth_cost
      * toll_booth_penalty
      * country_crossing_cost
      * country_crossing_penalty
    * bus
    * auto_shorter 
    * pedestrian
      * walking_speed
      * walkway_factor
      * alley_factor
      * driveway_factor
      * step_penalty
      * transit_start_end_max_distance
      * transit_transfer_max_distance
    * multimodal
      * use_bus
      * use_rail
      * use_transfers
      * transfer_cost
      * transfer_penalty
    * bicycle
      * maneuver_penalty
      * destination_only_penalty
      * alley_penalty
      * gate_cost
      * country_crossing_cost
      * country_crossing_penalty
      * bicycle_type
      * use_roads
  * locations [ ]
    * lat
    * lon
    * type
      * break
      * through
    * ...
* /optimized_route?
  * same as /route? except:
    * no multimodal costing
    * no date_time
* /source_to_targets?
  * same as /route? except:
* /many_to_one? /one_to_many? /many_to_many?
  * same as /route? except:
    * locations array order has differing meaning depending on which one
    * no multimodal costing
    * no date_time
    * no directions_options
* /isochrone?
  * same as /route? except:
    * location order doesnt have any meaning
    * no directions_options
* /height?
  * TODO:
