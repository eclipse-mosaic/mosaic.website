---
title: SUMO Configuration
linktitle: SUMO
toc: true
type: docs
draft: false
weight: 107
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

<span class="page_with_curl">:page_with_curl:</span> **Corresponding configuration file: `sumo/sumo_config.json`**



---------------------------------------
<a name="reference-sumo"></a>
## SUMO

Schema describing the JSON file structure for the SUMO configuration.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|visualizer|`boolean`|Starts the GUI interface of SUMO, if set to true (default: false).|No|None|`false`|
|updateInterval|`string`<br>`number`|The Interval after which positions are published. If defined as a number, then the default unit is ms. Alternatively this can be defined as a string to include the unit of measurement (e.g. '1 s'). Define the size of one simulation step in sumo.|No|None|`1000`|
|sumoConfigurationFile|`string`|Name of the main SUMO scenario configuration (*.sumocfg). If this member equals null, the SUMO ambassador will try to find a '.sumocfg' file.|No|None|None|
|exitOnInsertionError|`boolean`|If too many vehicles try to enter the simulation, SUMO might skip some vehicles and tries to enter them later again. This behavior can lead to wrong simulation results. This parameter defines, if the ambassador should try to continue the simulation in such cases. Quit SUMO, if an error occurs while inserting a new vehicle (e.g. due to high vehicle densities) (recommended: true).|No|None|`true`|
|additionalSumoParameters|`string`|Add additional parameters to the SUMO start command. Set a particular seed for the random number generator. By using different values you can have different but still reproducible simulation runs. Ignore possible waiting times by setting time-to-teleport to 0. This avoids unmoved vehicles (in our case also RSUs) being removed from simulation.|No|None|`--time-to-teleport 0 --seed 100000`|
|trafficFlowMeasurementWindowInS|`integer`|Defines the time window in seconds in which vehicle counts on induction loops should be aggregated to traffic flow. The value should be given in veh/h.|No|[1, +$\infty$]|`300`|
|timeGapOffset|`number`|This offset is added to all time-gap related parametrizations of vehicles (e.g. declaring vehicle types to SUMO, changing time-gap/reaction time during simulation). This could be helpful as IDM should be parametrized with lower time gaps to achieve specific time gap values.|No|None|`0`|
|subscribeToAllVehicles|`boolean`|If set to true all vehicles will be subscribed. If set to false only vehicles with applications mapped to them will be subscribed.|No|None|`true`|
|subscriptions|`string[]`|An optional list of subscriptions for each vehicle in the simulation. The less subscriptions given, the faster the simulation. Per default (if this list is set to null), all subscriptions are activated. Please note, that some components expect specific information, such as the road position. If this information is not subscribed, these components may fail. Possible values are: ["roadposition", "leader", "signals", "emissions", "trains"]|No|None|None|
|trafficSignLaneWidth|`number`|The default lane width to be used when adding traffic signs per lane (only relevant when using SUMO-GUI)|No|[0.1, +$\infty$]|`3.2`|
|highlights|`string[]`|Configure to highlight a vehicle in the GUI if it's performing a route or lane change, e.g. for debugging purposes (only relevant when using SUMO-GUI).|No|None|None|
|additionalVehicleTypeParameters|`object`|Allows to configure specialised vType parameters, which can't be configured via mapping (e.g. parameters for the lane change model of vehicles). The key of this object has to match with the name of a protype defined in the mapping configuration.|No|None|None|

**Further property restrictions:**  
<a name="restriction-sumoadditionalvehicletypeparameters"></a> 
### SUMO.additionalVehicleTypeParameters

* **Type of each property**: `additionalVehicleType`


---------------------------------------
<a name="reference-additionalvehicletype"></a>
## additionalVehicleType

Object to define a specialised vType.

**The following additional properties are allowed:**  
* [additionalParameter](#reference-additionalParameter) 



---------------------------------------
<a name="reference-additionalparameter"></a>
## additionalParameter

The vehicle parameter.

