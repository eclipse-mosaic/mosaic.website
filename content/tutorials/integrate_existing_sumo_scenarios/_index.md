---
title: Integrate Existing SUMO Scenarios
linktitle: integrate_existing_sumo_scenarios
toc: false
type: tutorials
date: "2021-01-02T00:00:00+01:00"
draft: false
pagination_prev: tiergarten_traffic_lights
pagination_next: lust
---

{{% alert note %}}
All files you need for this tutorial are included in the Eclipse MOSAIC zip file:  
**[Download Eclipse MOSAIC](/download)**
{{% /alert %}}

{{% alert learning_objectives %}} This tutorial will demonstrate how to simultaneously define traffic demand from MOSAIC's mapping-file and
SUMOs route-files and introduce handling of bicycles and busses. This tutorial won't introduce the basics of traffic definitions in
MOSAIC/SUMO, so if you are not familiar with the topic check out our other tutorials. After this tutorial you will be able to:

* Couple existing SUMO scenarios with MOSAIC
* Utilize different transport modalities in MOSAIC
{{% /alert %}}

## Overview

In this scenario several traffic participants meet at a junction (Sievekingplatz, Hamburg). The network and some of these participants
are already defined in the SUMO scenario. First we will have a look at the existing scenario and afterwards explain how we can extend
it. We will add some additional traffic and also equip vehicles with applications.

{{< figure src="images/integrate_existing_sumo_scenarios-overview.png" title="Overview of the Sievekingplatz scenario" numbered="true" >}}

## The SUMO scenario
In the `sumo`-directory of the scenario you will find the following structure:
```plaintext
└─ sumo
   ├─ sievekingplatz.bus.add.xml ................... Contains information about bus stops 
   ├─ sievekingplatz.net.xml ....................... The network of the scenario
   ├─ sievekingplatz.rou.xml ....................... Contains traffic demand definitions
   ├─ sievekingplatz.sumocfg ....................... SUMOs main configuration file
   └─ sumo_config.json ............................. MOSAIC configuration file for SUMO
```

The `sievekingplatz.sumocfg` is the basic configuration file for the SUMO scenario. It links to the net-file, the route-file and 
can also be run without using MOSAIC. 
Let us have a look at the route-file, it defines two `vTypes` and then some traffic demand. Concretely it defines 2 busses, 2 bicycles
and 1 vehicle.
````xml

<routes xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/routes_file.xsd">
    <vType id="Bus" vClass="bus" length="20"/>
    <vType id="Bike" jmIgnoreFoeProb="1.0" impatience="1.0" vClass="bicycle"/>

    <vehicle id="bus_0" type="Bus" depart="0.00">
        <route edges="139083727#2 370073260#0 370073258#0 24400729#1">
            <stop busStop="busstop" duration="60"/>
        </route>
    </vehicle>
    <vehicle id="bus_1" type="Bus" depart="0.00">
        <route edges="139083727#1 139083727#2 370073260#0 370073258#0 24400729#1">
            <stop busStop="busstop" duration="60"/>
        </route>
    </vehicle>

    <vehicle id="vehicle_straight" depart="0.00">
        <route edges="139083727#1 139083727#2 370073260#0 370073258#0 -4919237#3"/>
    </vehicle>

    <vehicle id="bike_left" type="Bike" depart="0.00" departPos="11">
        <route edges="gneE3 370073260#0 370073258#0 -4919237#3"/>
    </vehicle>
    <vehicle id="bike_right" type="Bike" depart="5.00">
        <route edges="624358653#1 624358654#0 624358654#1 gneE4 458455265"/>
    </vehicle>
</routes>
````

You can try running the scenario using the following command:

````plaintext
sumo -c <path-to-config>/sievenkingplatz.sumocfg
````

## Setting up the MOSAIC scenario
Now lets set up a complete MOSAIC scenario starting from the SUMO scenario we have so far. 
We will go along the following steps and explain everything in detail:
1. Creating the scenario database.
2. (Optional) Loading the routes into the database.
3. Create the MOSAIC scenario folder structure.
4. Set up the `scenario_config.json`
5. Have a look at some additional SUMO configuration.

### 1. Creating the scenario database
To create the scenario database we will use the [scenario-convert](/docs/scenarios/scenario_convert)
script, that comes bundled with the MOSAIC extended version from the  [download section](/download). 
Start by calling: 
````plaintext
scenario-convert.sh --sumo2db -i sievekingplatz.net.xml
````
This will create a database called `sievekingplatz.db` and load the network into the database.

### 2. Loading the routes into the database
Next we will use the same command to load the routes of the route-file into the database.
This is optional, but necessary if you want to add vehicles along the routes of your SUMO route-file.
````plaintext
scenario-convert.sh --sumo2db -i sievekingplatz.rou.xml -d sievekingplatz.db
````
Note, that we also have to specify the database here.

### 3. Create the MOSAIC scenario folder structure
MOSAIC requires a special folder structure to organize the configuration of different federates. If you haven't dealt with MOSAIC
scenarios yet have a look [here](/docs/scenarios/scenario_configuration).
Start by creating a following folder structure representing the most basic set-up for a MOSAIC scenario:
```plaintext
└─ sievekingplatz
   ├─ application
   ├─ mapping
   └─ sumo
```
Next copy the database into the `application`-directory and all SUMO files into the `sumo`-directory.
Additionally, you can create the configuration file `mapping_config.json` in the `mapping`-directory, we will fill this file later on.

### 4. Set up the `scenario_config.json`
Create a file called `scenario_config.json` in the parent directory of you scenario, this is the main configuration for your scenario.
The folder structure should now look as following:
```plaintext
└─ sievekingplatz
   ├─ application
   |  └─ sievekingplatz.db
   ├─ mapping
   |  └─ mapping_config.json
   ├─ sumo
   |  ├─ sievekingplatz.bus.add.xml 
   |  ├─ sievekingplatz.net.xml
   |  ├─ sievekingplatz.rou.xml
   |  ├─ sievekingplatz.sumocfg
   |  └─ sumo_config.json
   └─ scenario_config.json
```
Now it's time to fill out the `scenario_config.json`. For now, you can copy the following configuration:
````json
{
    "simulation": {
        "id": "Sievekingplatz",
        "duration": "200s",
        "randomSeed": 212323853,
        "projection": {
            "centerCoordinates": {
                "latitude": 52.63,
                "longitude": 13.56
            },
            "cartesianOffset": {
                "x": -563984.16,
                "y": -5933566.87
            }
        },
        "network": {
            "netMask": "255.255.0.0",
            "vehicleNet": "10.1.0.0",
            "rsuNet": "10.2.0.0",
            "tlNet": "10.3.0.0",
            "csNet": "10.4.0.0",
            "serverNet": "10.5.0.0",
            "tmcNet": "10.6.0.0"
        }
    },
    "federates": {
        "application": true,
        "cell": false,
        "environment": false,
        "sns": false,
        "ns3": false,
        "omnetpp": false,
        "sumo": true
    }
}
````
> Note: The `cartesianOffset` values are taken from the net-file and can generally just be copied over.

### 5. Additional SUMO configuration
In the `sievekingplatz.sumocfg` the `step-length` is defined, this parameter will, by default, be ignored in MOSAIC and has to be
set in the `sumo_config.json` (located in the `sumo`-directory) to ensure the same behaviour. 
(SUMO uses seconds as unit and MOSAIC milliseconds)

**`sievekingplatz.sumocfg`**:
````xml
    ...
    <time>
        <step-length value="0.1"/>
    </time>
    ...    
````

**`sumo_config.json`**:
````json
{
    "updateInterval": 100
}
````

## Additional Traffic Demand from MOSAIC
Now that we set up the basic scenario structure, we can continue by creating some additional traffic demand.
Let's try to add 3 additional vehicles using a flow definition in the `mapping_config.json`-file.
We first define the prototype `MappingCar` for our new vehicles. Afterwards we define a spawner for three additional vehicles and also
equip them with the `HelloWorldApp`, which we will have a look at next.  
````json
{
    "prototypes": [
        {
            "name": "MappingCar"
        },
        ...
    ],
    "vehicles": [
        {
            "startingTime": "10.0 s",
            "route": "3",
            "targetFlow": 1200,
            "maxNumberVehicles": 3,
            "lanes": [ 1, 2 ],
            "types": [
                {
                    "name": "MappingCar",
                    "applications": [ "org.eclipse.mosaic.app.tutorial.eventprocessing.sampling.HelloWorldApp" ]
                }
            ]
        }
    ]
}
````
> This isn't shown here, but it is also possible to define bikes and busses in the `mapping_config.json`. In order to do this you have
> to set the according `"vehicleClass"`-attribute in the prototype (see `org.eclipse.mosaic.lib.enums.VehicleClass`).

We can now start the scenario using the MOSAIC start script and will see both the vehicles from SUMO and the ones from MOSAIC.

## Equipping the vehicles with applications

We will now extend our scenario by equipping the participants with a simple application:
| Application | Description  |
| ----------- | ------------ |
| `org.eclipse.mosaic.app.tutorial.eventprocessing.sampling.`**`HelloWorldApp`** | This app gets mapped on all participants and is just used to showcase that they're handled properly by the `ApplicationAmbassador` |

We do this by mapping the application to vTypes defined in the SUMO route-file. This functions analogously to how you would do it when
only using the mapping-file, though you cannot map apps on single vehicles but only their vTypes. 
Additionally, there are some more things to consider:
* The `"DEFAULT_VEHTYPE"` prototype is the name of the default `vType` in SUMO. So all vehicle definitions, missing a `type`-parameter will
  get the `HelloWorldApp` mapped to them.
* The `Bike` prototype additionally has a `"weight"`-attribute. This controls the percentage of vehicles, that will get the apps mapped to them.
* The `Bus` type is configured in the `typeDistribution` section. This is very similar to the `Bike` definition, with the possibility to define multiple application configurations to randomly choose from.
* The vehicle spawner defines the `"lanes"`-attribute, since lane 0 is a bus-lane.
* You can't specify spawners for any of the types defined in SUMO as this would lead to MOSAIC writing the type again and thereby crashing
  the simulation.
* You can however define vehicles in the route-file using the type `MappingCar`, as this is written in a separate file beforehand.

```json
{
  "prototypes": [ 
    {
      "name": "MappingCar"
    },
    {
      "name": "Bike",
      "weight": 0.8,
      "applications": [ "org.eclipse.mosaic.app.tutorial.eventprocessing.sampling.HelloWorldApp" ]
    },
    {
      "name": "DEFAULT_VEHTYPE",
      "applications": [ "org.eclipse.mosaic.app.tutorial.eventprocessing.sampling.HelloWorldApp" ]
    },
    {
      "name": "Bus",
      "applications": [ "org.eclipse.mosaic.app.tutorial.eventprocessing.sampling.HelloWorldApp" ]
    }
  ],
  "typeDistributions": {
    "Bus": [
      {
        "name": "Bus",
        "weight": 0.4
      },
      {
        "name": "Bus",
        "weight": 0.6,
        "applications": [ ]
      }
    ]
  },
  ...
}
```

{{% alert note %}}
The actual vehicle properties such as acceleration or maximum speed of vehicles defined in the SUMO scenario, **can not** be overriden in the mapping configuration. Please adjust these values in the `vType` section in the SUMO scenario files.
{{% /alert %}}

## Simulation and Results
We are done building our scenario and can now run it using MOSAIC's start script.
```
mosaic.<sh|bat> -c <path-to-scenario>/scenario_config.json
```
The figure below is a snapshot of the simulation showing the different types of participants. Vehicles defined in SUMO
will have the id defined in the route-file, vehicles from mapping will follow the pattern `veh_<number>`. In MOSAIC all
vehicles are assigned an id, which follows this pattern. This is why the number at times might seem arbitrary, in reality
it is counted upwards beginning from 0, in the order of spawning.

{{< figure src="images/integrate_existing_sumo_scenarios-simulation.png" title="Overview of the Sievekingplatz scenario during simulation including type parameters" numbered="true" >}}

### Logs  
We'll have a quick look at some logs of the applications to see, that everything functions properly.

**`veh_0/HelloWorldApp.log`**:
````log
INFO  - Hello World! I'm a Bus. (at simulation time 0.200,000,000 s)
INFO  - I'm still here! (at simulation time 1.200,000,000 s)
...
INFO  - I'm still here! (at simulation time 98.200,000,000 s)
INFO  - Bye bye World (at simulation time 99.100,000,000 s)
 ````
This is the log of the first bus added, which can be seen in the first logged message.
One thing to note is that it will take 2 simulation steps for the `ApplicationAmbassador` to startup
apps on SUMO vehicles. This is due to how things are handled internally.

**`veh_5/HelloWorldApp.log`**:
````log
INFO  - Hello World! I'm a MappingCar. (at simulation time 10.100,000,000 s)
INFO  - I'm still here! (at simulation time 11.100,000,000 s)
...
INFO  - I'm still here! (at simulation time 24.100,000,000 s)
INFO  - Bye bye World (at simulation time 24.300,000,000 s)
````
This is the log of the first car from the `mapping_config.json`.

## Summary
It should be fairly simple to transfer the shown methods to an arbitrary SUMO scenario. This is especially useful if you already have
existing scenarios that you want to extend with MOSAIC's application and communication capabilities as you don't have to worry about
configuring the traffic demand again.