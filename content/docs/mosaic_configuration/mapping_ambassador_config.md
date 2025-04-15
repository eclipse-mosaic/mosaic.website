---
title: Mapping Ambassador Configuration
linktitle: Mapping
toc: true
type: docs
draft: false
weight: 101
menu:
  docs:
    parent: mosaic_configuration
---

{{% alert tip %}}
You can use a {{< link title="Readable Unit" href="/docs/mosaic_configuration/#readable-units">}} anywhere where you
have the option of:
- `number` **or** `string`
- `integer` **or** `string`
{{% /alert %}}

---

<span class="page_with_curl">:page_with_curl:</span> **Corresponding configuration file: `mapping/mapping_config.json`**



---------------------------------------
<a name="reference-mapping"></a>
## Mapping

This schema describes the JSON file structure for the mapping configuration, which is used to define simulation entities.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|config|[`config`](#reference-config)|Object to define additional configuration options for the mapping|No|None|None|
|prototypes|[`prototype[]`](#reference-prototype)|Array of prototypes, which can complete the definitions of other objects. This can be used to re-use certain configurations. All possible properties of objects are available. Only the ones needed will be used (for example an RSU does not have a length, so this property would be ignored).|No|None|None|
|typeDistributions|[`typeDistribution`](#reference-typedistribution)|Object to define the distribution of prototypes to reuse in vehicle spawners. A typeDistribution is referenced by it's attribute name, which can be set to any valid string.|No|None|None|
|vehicles|[`vehicle[]`](#reference-vehicle)|Array of vehicles to be spawned in the simulation. This property describes the vehicles populating the simulation. It is possible to create a single vehicle (maxNumberVehicles should be '1' in that case) or a stream of one or multiple vehicles. The type(s) are defined in the field types. When more than one type is defined the weights in the prototype can be used to balance them against each other.|No|None|None|
|agents|[`agent[]`](#reference-agent)|Array of agents to be spawned in the simulation.|No|None|None|
|matrixMappers|[`matrixMapper[]`](#reference-matrixmapper)|Array of items to define additional traffic that will be spawned using OD-matrices.|No|None|None|
|rsus|[`rsu[]`](#reference-rsu)|Array of Road Side Units (RSUs). For RSUs only applications can be defined along with the position.|No|None|None|
|servers|[`server[]`](#reference-server)|Array of servers. Servers are a form of units that have no geographical location. The network properties of a server can be defined in the network.json-configuration in the cell-module.|No|None|None|
|tmcs|[`tmc[]`](#reference-tmc)|Array of Traffic Management Centers (TMCs). TMCs are specialized forms of servers having direct access to data collected by induction loops and lane area detectors. The network properties of a TMC can be defined in the network.json-configuration in the cell-module.|No|None|None|
|trafficLights|[`trafficLights[]`](#reference-trafficlights)|Array of prototypes for traffic lights. Since it is a traffic light only applications can be defined. Traffic light prototypes can be distributed among all traffic lights of an application by weight or assigned to specific traffic lights by using the ID of traffic light groups as reference.|No|None|None|
|chargingStations|[`chargingStation[]`](#reference-chargingstation)|Array of electric vehicle charging stations. An infrastructure which provides one or several electric vehicle charging spots to supply electric energy for charging electric vehicles.|No|None|None|

**Further property restrictions:**  
<a name="restriction-mappingtypedistributions"></a> 
### Mapping.typeDistributions

* **Type of each property**: `array`


---------------------------------------
<a name="reference-config"></a>
## config

Object to define additional configuration options for the mapping

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|start|`string`<br>`number`|Defines the point in time (in seconds) to start spawning vehicles. If not set (default), all vehicles will be spawned according to the vehicles configuration.|No|None|None|
|end|`string`<br>`number`|Defines the point in time (in seconds) to end spawning vehicles. If not set (default), all vehicles will be spawned according to the vehicles configuration or until the simulation ends.|No|None|None|
|scaleTraffic|`number`|Scales the traffic by the given factor. E.g. 2.0 would double the number of spawned vehicles|No|[0, +$\infty$]|`1`|
|fixedOrder|`boolean`|Determines if the type selection of a spawning vehicle follows a fixed order or stochastic model. When set to true the spawning-process will choose exactly the same type order with every execution. When set to false (default) the order of types follows a stochastic manner and will be different each time depending on the set random seed, however, selected weights may be reached more slowly.|No|None|`false`|
|adjustStartingTimes|`boolean`|If set to true and if the parameter start is set, the starting times of each spawner is adjusted accordingly, so that we shouldn't wait in case that the simulation starting time and spawner starting time are widely spread out. All spawners before start will be completely ignored then.|No|None|`false`|
|randomizeFlows|`boolean`|If set to true, all flow definitions defined by vehicle spawners with more than one vehicle resulting in slightly randomized departure times. The specified `targetFlow` of the vehicle spawner is kept.|No|None|`false`|
|randomizeStartingTimes|`boolean`|If set to true, the starting times of all vehicle spawner definitions are randomized by +-60seconds.|No|None|`false`|
|randomizeWeights|`boolean`|If set to true, the configured weights of all types are slightly randomized by +-1% of the sum of all weights.|No|None|`false`|



---------------------------------------
<a name="reference-prototype"></a>
## prototype

Object to define a prototype, which can complete the definitions of other objects. This can be used to re-use certain configurations. All possible properties of objects are available. Only the ones needed will be used (for example an RSU does not have a length, so this property would be ignored).

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|name|`string`|The name of this prototype is used to match it against other objects.| &#10003; Yes|None|None|
|group|`string`|The group name is used for (statistical) evaluation purposes with the StatisticOutput and ITEF. It allows to summarize multiple prototype entities.|No|None|None|
|color|`string`|The color of the vehicle for visualization purposes.|No|None|None|
|accel|`number`|Acceleration in m/s^2.|No|(0, +$\infty$]|None|
|decel|`number`|Deceleration in m/s^2.|No|(0, +$\infty$]|None|
|length|`string`<br>`number`|Length of the vehicle. If defined as a number, then the default unit is m. Alternatively this can be defined as a string to specify the unit of measurement (e.g. '500 cm').|No|(0, +$\infty$]|None|
|maxSpeed|`string`<br>`number`|Maximal speed. If defined as a number, then the default unit is m/s. Alternatively this can be defined as a string to include the unit of measurement (e.g. '50 kmh').|No|None|None|
|speedFactor|`number`|The speed factor of the vehicle to apply to speed limits. E.g., with a value of 1.1 the vehicle would exceed the speed limit of an edge by 10 percent.|No|[0, +$\infty$]|None|
|minGap|`string`<br>`number`|Distance in meter between front bumper of a vehicle and the back bumper of its leader in a traffic jam. If defined as a number, then the default unit is m. Alternatively this can be defined as a string to include the unit of measurement (e.g. '300 cm').|No|(0, +$\infty$]|None|
|sigma|`number`|Driver imperfection. This is a parameter of the car-following model.|No|[0, 1]|None|
|tau|`number`|Driver reaction time in seconds. This is a parameter of the car-following model.|No|[0, +$\infty$]|None|
|weight|`number`|The weight is used to distribute objects between multiple types. All weights do NOT have to add up to 1 or 100. (Example: A vehicle spawner defining a traffic stream contains two prototypeDeserializers with the weights being 4 and 6. The resulting traffic stream will consist to 40% of the one type and 60% of the other)|No|[0, +$\infty$]|None|
|deviations|[`deviations`](#reference-deviations)|Object to define a standard deviation of single parameters of a specific prototype. For each spawned vehicle, a variation of the prototype will be generated, if at least one of the following parameters is larger than 0.|No|None|None|
|laneChangeMode|`string`|Predefined modes adjusting the lane change behavior.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-prototypelanechangemode)|None|
|speedMode|`string`|Predefined modes adjusting the speed appliance behavior.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-prototypespeedmode)|None|
|vehicleClass|`string`|Class of the vehicle. The classes are used in lane definitions and allow/disallow the use of lanes for certain vehicle types (e.g. a taxi lane).|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-prototypevehicleclass)|None|
|applications|`string[]`|The applications to be used for this object.|No|None|None|

**Further property restrictions:**  
<a name="restriction-prototypelanechangemode"></a> 
### prototype.laneChangeMode

* **Allowed values**:
   * `DEFAULT`
   * `OFF`
   * `CAUTIOUS`
   * `COOPERATIVE`
   * `AGGRESSIVE`
   * `PASSIVE`
<a name="restriction-prototypespeedmode"></a> 
### prototype.speedMode

* **Allowed values**:
   * `DEFAULT`
   * `CAUTIOUS`
   * `NORMAL`
   * `AGGRESSIVE`
   * `SPEEDER`
<a name="restriction-prototypevehicleclass"></a> 
### prototype.vehicleClass

* **Allowed values**:
   * `Unknown`
   * `Car`
   * `LightGoodsVehicle`
   * `HeavyGoodsVehicle`
   * `PublicTransportVehicle`
   * `EmergencyVehicle`
   * `WorksVehicle`
   * `ExceptionalSizeVehicle`
   * `VehicleWithTrailer`
   * `HighSideVehicle`
   * `MiniBus`
   * `Taxi`
   * `ElectricVehicle`
   * `AutomatedVehicle`
   * `Bicycle`
   * `Motorcycle`
   * `HighOccupancyVehicle`


---------------------------------------
<a name="reference-deviations"></a>
## Prototype Parameter Deviations

Object to define a standard deviation of single parameters of a specific prototype. For each spawned vehicle, a variation of the prototype will be generated, if at least one of the following parameters is larger than 0.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|length|`number`|The standard deviation to apply on the length of the prototype.|No|[0, +$\infty$]|None|
|width|`number`|The standard deviation to apply on the width of the prototype.|No|[0, +$\infty$]|None|
|height|`number`|The standard deviation to apply on the height of the prototype.|No|[0, +$\infty$]|None|
|minGap|`number`|The standard deviation to apply on the minimum gap of the prototype.|No|[0, +$\infty$]|None|
|maxSpeed|`number`|The standard deviation to apply on the maximum speed of the prototype.|No|[0, +$\infty$]|None|
|speedFactor|`number`|The standard deviation to apply on the speed factor of the prototype.|No|[0, +$\infty$]|None|
|accel|`number`|The standard deviation to apply on the acceleration value of the prototype.|No|[0, +$\infty$]|None|
|decel|`number`|The standard deviation to apply on the deceleration value of the prototype.|No|[0, +$\infty$]|None|
|tau|`number`|The standard deviation to apply on the desired headway time (tau) of the prototype.|No|[0, +$\infty$]|None|



---------------------------------------
<a name="reference-typedistribution"></a>
## typeDistribution

Object to define the distribution of prototypes to reuse in vehicle spawners. A typeDistribution is referenced by it's attribute name, which can be set to any valid string.

**The following additional properties are allowed:**  
* array[[prototype](#reference-prototype)] 



---------------------------------------
<a name="reference-vehicle"></a>
## vehicle

Object to define vehicles to be spawned in the simulation. This property describes the vehicles populating the simulation. It is possible to create a single vehicle (maxNumberVehicles should be '1' in that case) or a stream of one or multiple vehicles. The types (or type) are defined in the field types. When more than one type is defined the weights in the prototype can be used to balance them against each other.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|startingTime|`string`<br>`number`|Time in seconds at which the first vehicle will be created.|No|None|None|
|maxTime|`string`<br>`number`|Simulation time in seconds at which no more vehicles will be created.|No|None|None|
|targetFlow|`number`|Density of vehicles per hour. Vehicles will be spawned uniformly.|No|[0, +$\infty$]|`600`|
|maxNumberVehicles|`number`|Maximum number of vehicles to be created from this source.|No|[0, +$\infty$]|None|
|departSpeed|`string`<br>`number`|The speed at which the vehicle is supposed to depart. If defined as a number, then the default unit is m/s. Alternatively this can be defined as a string to include the unit of measurement (e.g. '10 kmh'). Depending on the simulator this value may only be used if departSpeedMode is set to PRECISE.|No|None|None|
|departSpeedMode|`string`|The depart speed mode determines the vehicle's speed at insertion.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-vehicledepartspeedmode)|`MAXIMUM`|
|laneSelectionMode|`string`|The lane selection mode chooses the lane for the next departing vehicle.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-vehiclelaneselectionmode)|`DEFAULT`|
|spawningMode|`string`|Adjusts the departure time of individual vehicles.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-vehiclespawningmode)|`CONSTANT`|
|departConnectionIndex|`number`|The index of the connection of the route where the vehicle will start on.|No|[0, +$\infty$]|`0`|
|pos|`number`|Position within the connection of the route where the vehicle(s) should be spawned.|No|[0, +$\infty$]|`0`|
|route|`string`|Route that the vehicle(s) should use. If an origin and a destination are specified this route will be treated as a preference (i.e. it will be selected if it connects the two points in question).|No|None|None|
|lanes|`number[]`|Array of numbers to define the lanes to be used. The vehicles will be evenly distributed among the given lanes. When no value is given lane zero will be used for all vehicles.|No|None|None|
|types|[`prototype[1-*]`](#reference-prototype)|List of possible vehicle types to be spawned. In this list you can simply refer to an existing prototype by its name attribute to include everything defined there. You can also overwrite every attribute of the prototype. If you don't have an existing prototype the definitions found here will be used as the prototype definition itself.|No|None|None|
|typeDistribution|`string`|Identifier of the typeDistribution which defines the distribution of vehicle types.|No|None|None|
|destination|[`geoCircle`](#reference-geocircle)|Object to define an immutable pair of a geoPoint center position and a radius in meters.| &#10003; Yes|None|None|
|origin|[`geoCircle`](#reference-geocircle)|Object to define an immutable pair of a geoPoint center position and a radius in meters.| &#10003; Yes|None|None|

**Further property restrictions:**  
<a name="restriction-vehicledepartspeedmode"></a> 
### vehicle.departSpeedMode

* **Allowed values**:
   * `PRECISE`
   * `RANDOM`
   * `MAXIMUM`
<a name="restriction-vehiclelaneselectionmode"></a> 
### vehicle.laneSelectionMode

* **Allowed values**:
   * `DEFAULT`
   * `ROUNDROBIN`
   * `ROUNDROBIN_HIGHWAY`
   * `HIGHWAY`
   * `RANDOM`
   * `FREE`
   * `ALLOWED`
   * `BEST`
   * `FIRST`
<a name="restriction-vehiclespawningmode"></a> 
### vehicle.spawningMode

* **Allowed values**:
   * `CONSTANT`
   * `GROW`
   * `POISSON`
   * `SHRINK`
   * `GROW_AND_SHRINK`
   * `GROW_EXPONENTIAL`
   * `SHRINK_EXPONENTIAL`
   * `GROW_AND_SHRINK_EXPONENTIAL`
   * `INSTANT`


---------------------------------------
<a name="reference-geocircle"></a>
## geoCircle

Object to define an immutable pair of a geoPoint center position and a radius in meters.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|center|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|radius|`number`|Radius of the circle.| &#10003; Yes|[0, +$\infty$]|None|



---------------------------------------
<a name="reference-geopoint"></a>
## GeoPoint

Object to define geographical point coordinates.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|longitude|`number`|East-west position of a point on earth.| &#10003; Yes|[-180, 180]|None|
|latitude|`number`|North-south position of a point on earth.| &#10003; Yes|[-90, 90]|None|



---------------------------------------
<a name="reference-agent"></a>
## agent

An agent has an origin and destination point. The trip selection is done via mapped applications.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|startingTime|`string`<br>`number`|Time in seconds at which the agent will be created.| &#10003; Yes|None|None|
|origin|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|destination|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|name|`string`|Used to be matched with a prototype. If a prototype name matches this name, all properties not set in this object will be overwritten by those defined in the prototype.|No|None|None|
|group|`string`|The group name is used for (statistical) evaluation purposes with the StatisticOutput and ITEF. It allows to summarize multiple agent entities.|No|None|None|
|applications|`string[]`|Array of strings that specifies the applications to be used for this object.|No|None|None|
|walkingSpeed|`string`<br>`number`|Speed which is used for walking, in m/s.|No|None|None|



---------------------------------------
<a name="reference-matrixmapper"></a>
## matrixMapper

Object to define a mapper for an Origin-Destination (OD) matrix. The mapper contains a list of points (with varying radius) and a matrix (arrays) of flow values. It creates a series of conventional vehicles spawners from the specified data.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|points|[`odPoint[]`](#reference-odpoint)|Array of odPoints that can be referenced from the OD-matrix.| &#10003; Yes|None|None|
|types|[`prototype[]`](#reference-prototype)|Array of prototypes to define the vehicles that should be spawned.|No|None|None|
|odValues|`array[]`|Values for the OD-matrix. Unit should be vehicles/hour.| &#10003; Yes|None|None|
|startingTime|`string`<br>`number`|Time in seconds at which the first vehicle will be created.|No|None|None|
|maxTime|`string`<br>`number`|Simulation time in seconds at which no more vehicles will be created.|No|None|None|
|departSpeedMode|`string`|The depart speed mode determines the vehicle's speed at insertion.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-matrixmapperdepartspeedmode)|`MAXIMUM`|
|laneSelectionMode|`string`|The lane selection mode chooses the lane for the next departing vehicle.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-matrixmapperlaneselectionmode)|`DEFAULT`|

**Further property restrictions:**  
<a name="restriction-matrixmapperdepartspeedmode"></a> 
### matrixMapper.departSpeedMode

* **Allowed values**:
   * `PRECISE`
   * `RANDOM`
   * `MAXIMUM`
<a name="restriction-matrixmapperlaneselectionmode"></a> 
### matrixMapper.laneSelectionMode

* **Allowed values**:
   * `DEFAULT`
   * `ROUNDROBIN`
   * `ROUNDROBIN_HIGHWAY`
   * `HIGHWAY`
   * `RANDOM`
   * `FREE`
   * `ALLOWED`
   * `BEST`
   * `FIRST`


---------------------------------------
<a name="reference-odpoint"></a>
## odPoint

Object to define a point that can be referenced from an OD-matrix.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|name|`string`|The name of the point. This identifier is used to reference the odPoint.| &#10003; Yes|None|None|
|position|[`geoCircle`](#reference-geocircle)|Object to define an immutable pair of a geoPoint center position and a radius in meters.| &#10003; Yes|None|None|



---------------------------------------
<a name="reference-rsu"></a>
## rsu

Object to define a Road Side Unit (RSU). For RSUs only applications can be defined along with the position.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|position|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|name|`string`|Used to be matched with a prototype. If a prototype name matches this name, all properties not set in this object will be overwritten by those defined in the prototype.|No|None|None|
|group|`string`|The group name is used for (statistical) evaluation purposes with the StatisticOutput and ITEF. It allows to summarize multiple rsu entities.|No|None|None|
|applications|`string[]`|Array of strings that specifies the applications to be used for this object. If none are specified, none are used|No|None|None|



---------------------------------------
<a name="reference-server"></a>
## server

Object to define a server. Servers are a form of units that have no geographical location. The network properties of a server can be defined in the network.json-configuration in the cell-module.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|name|`string`|Used to be matched with a prototype. If a prototype name matches this name, all properties not set in this object will be overwritten by those defined in the prototype.|No|None|None|
|group|`string`|The group name is used to match with configurations in the network.json-configuration in the cell-module.|No|None|None|
|applications|`string[]`|The applications to be used for this object. If none are specified, none are used.|No|None|None|



---------------------------------------
<a name="reference-tmc"></a>
## tmc

Object to define a Traffic Management Center (TMCs). TMCs are specialized forms of server directly communicating with induction loops and lane area detectors. The network properties of a TMC can be defined in the network.json-configuration in the cell-module.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|name|`string`|Used to be matched with a prototype. If a prototype name matches this name, all properties not set in this object will be overwritten by those defined in the prototype.|No|None|None|
|group|`string`|The group name is used to match with configurations in the network.json-configuration in the cell-module.|No|None|None|
|applications|`string[]`|The applications to be used for this object. If none are specified, none are used.|No|None|None|
|inductionLoops|`string[]`|The induction loops the TMC shall be matched with. If none are specified, none are used.|No|None|None|
|laneAreaDetectors|`string[]`|The lane area detectors the TMC shall be matched with. If none are specified, none are used.|No|None|None|



---------------------------------------
<a name="reference-trafficlights"></a>
## trafficLights

Object to define a prototype for a traffic light. Since it is a traffic light only applications can be defined. Traffic light prototypes can be distributed among all traffic lights of an application by weight or assigned to specific traffic lights by using the ID of traffic light groups as reference.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|name|`string`|Used to be matched with a prototype. If a prototype name matches this name, all properties not set in this object will be overwritten by those defined in the prototype.|No|None|None|
|tlGroupId|`string`|The ID of a traffic light group. This property is used to map applications to specific traffic lights.|No|None|None|
|group|`string`|The group name is used for (statistical) evaluation purposes with the StatisticOutput and ITEF. It allows to summarize multiple trafficLights entities.|No|None|None|
|weight|`number`|The weight is used to distribute traffic lights between multiple default types. If tlGroupId is not set, then the default value is 1, otherwise 0. All weights do NOT have to add up to 1 or 100.|No|[0, +$\infty$]|None|
|applications|`string[]`|The applications to be used for this object. If none are specified, none are used|No|None|None|



---------------------------------------
<a name="reference-chargingstation"></a>
## chargingStation

Object to define an electric vehicle charging station. An infrastructure which provides one or several electric vehicle charging spots to supply electric energy for charging electric vehicles.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|position|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|group|`string`|The group name is used for (statistical) evaluation purposes with the StatisticOutput and ITEF. It allows to summarize multiple chargingStation entities.|No|None|None|
|name|`string`|Used to be matched with a prototype. If a prototype name matches this name, all properties not set in this object will be overwritten by those defined in the prototype.|No|None|None|
|chargingSpots|[`chargingSpot[]`](#reference-chargingspot)|List of the electric vehicle charging spots associated with this electric vehicle charging station.| &#10003; Yes|None|None|
|applications|`string[]`|The application to be used for this object.|No|None|None|



---------------------------------------
<a name="reference-chargingspot"></a>
## chargingSpot

Object to define an electric vehicle charging spot.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|chargingType|`string`|The type of this electric vehicle charging spot.| &#10003; Yes|Enum[<i class="fas fa-info-circle"></i>](#restriction-chargingspotchargingtype)|None|
|maxVoltage|`number`|Maximum voltage available at this charging spot.| &#10003; Yes|[0, +$\infty$]|None|
|maxCurrent|`number`|Maximum current available at this charging spot.| &#10003; Yes|[0, +$\infty$]|None|

**Further property restrictions:**  
<a name="restriction-chargingspotchargingtype"></a> 
### chargingSpot.chargingType

* **Allowed values**:
   * `AC_1_PHASE`
   * `AC_3_PHASE`
   * `DC`
