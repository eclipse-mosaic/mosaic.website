---
title: Scenario Configuration
linktitle: Scenario
toc: true
type: docs
draft: false
weight: 100
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

<span class="page_with_curl">:page_with_curl:</span> **Corresponding configuration file: `scenario_config.json`**



---------------------------------------
<a name="reference-scenario"></a>
## Scenario

Schema describing the JSON file structure for the main scenario configuration.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|simulation|[`simulation`](#reference-simulation)|Basic properties of the simulation scenario.| &#10003; Yes|None|None|
|federates|`object`|Defines the list of ambassadors/simulators which are used by the simulation. For each simulator (referenced by its id) a boolean value must be given, which enables or disables the simulator.| &#10003; Yes|None|None|

**Further property restrictions:**  
<a name="restriction-scenariofederates"></a> 
### Scenario.federates

* **Type of each property**: `boolean`


---------------------------------------
<a name="reference-simulation"></a>
## simulation

Basic properties of the simulation scenario.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|id|`string`|The id or name of the simulation scenario.| &#10003; Yes|None|None|
|duration|`string`<br>`number`|The duration of the simulation. If defined as a number, then the unit to be applied is ns. Alternatively this can be defined as a string to include the unit of measurement (e.g. '300 s')| &#10003; Yes|None|None|
|randomSeed|`integer`|The random seed to apply. This influences the application mapping, the communication models, and any use of randomness inside of applications. If not set, the random number generator will be initialized without a seed resulting in different results for each run. Note that several other simulators (e.g. SUMO) come with their own random number generators which are not affected by this property.|No|None|None|
|projection|[`projection`](#reference-projection)|Configures the projection used to transform geographical coordinates (WGS84) to local cartesian coordinates used by various simulators (e.g. SUMO, or OMNeT++). The projection is based on UTM and must be adapted to the used traffic network.| &#10003; Yes|None|None|
|network|[`network`](#reference-network)|Defines the subnets (e.g. 10.0.0.0) used for generating IP addresses for each unit in the simulation. Each type of unit is configured with its own subnet. A default configuration is used, when left out from the configuration.|No|None|`default network`|



---------------------------------------
<a name="reference-projection"></a>
## Projection

Configures the projection used to transform geographical coordinates (WGS84) to local cartesian coordinates used by various simulators (e.g. SUMO, or OMNeT++). The projection is based on UTM and must be adapted to the used traffic network.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|centerCoordinates|[`geoPoint`](#reference-geopoint)|Geographic coordinates of the rough center of the map / playground used in this scenario. This is just used to determine the UTM zone used for any further projection.| &#10003; Yes|None|None|
|cartesianOffset|[`cartesianOffset`](#reference-cartesianoffset)|A cartesian offset which is added to the UTM transformation. In most cases, this must be the exact offset which can be found in the `location.netOffset` of the `*.net.xml` of the scenario.| &#10003; Yes|None|None|



---------------------------------------
<a name="reference-geopoint"></a>
## GeoPoint

Geographic coordinates of the rough center of the map / playground used in this scenario. This is just used to determine the UTM zone used for any further projection.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|longitude|`number`|East-west position of a point on earth.| &#10003; Yes|[-180, 180]|None|
|latitude|`number`|North-south position of a point on earth.| &#10003; Yes|[-90, 90]|None|



---------------------------------------
<a name="reference-cartesianoffset"></a>
## CartesianOffset

A cartesian offset which is added to the UTM transformation. In most cases, this must be the exact offset which can be found in the `location.netOffset` of the `*.net.xml` of the scenario.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|x|`number`|The value to add to the X coordinate after UTM transformation| &#10003; Yes|None|None|
|y|`number`|The value to add to the Y coordinate after UTM transformation| &#10003; Yes|None|None|



---------------------------------------
<a name="reference-network"></a>
## Network Addressing

Defines the subnets (e.g. 10.0.0.0) used for generating IP addresses for each unit in the simulation. Each type of unit is configured with its own subnet. A default configuration is used, when left out from the configuration.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|netMask|`string`|The net mask used for IP generation.|No|None|`255.0.0.0`|
|vehicleNet|`string`|The subnet used for assigning IPs to vehicles.|No|None|`10.0.0.0`|
|rsuNet|`string`|The subnet used for assigning IPs to road side units.|No|None|`11.0.0.0`|
|tlNet|`string`|The subnet used for assigning IPs to traffic lights.|No|None|`12.0.0.0`|
|csNet|`string`|The subnet used for assigning IPs to charging stations.|No|None|`13.0.0.0`|
|serverNet|`string`|The subnet used for assigning IPs to servers.|No|None|`14.0.0.0`|
|tmcNet|`string`|The subnet used for assigning IPs to traffic management centers.|No|None|`15.0.0.0`|

