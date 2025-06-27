---
title: PHABMACS - PHysics Aware Behavior Modeling Advanced Car Simulator
linktitle: Vehicle - PHABMACS
toc: true
type: docs
draft: false
weight: 22
menu:
  docs:
    parent: simulators
---

The Physics Aware Behavior Modelling Advanced Car Simulator (PHABMACS) provides a sophisticated framework for testing ADAS within
a generated 3D environment. Vehicles are simulated using advanced models providing them with realistic driving dynamics
within individual traffic situations.

|                      |                                                                                                                      | |
|----------------------|----------------------------------------------------------------------------------------------------------------------|-|
| **Operating System** | GNU/Linux, Microsoft Windows, *currently no support for Apple Silicon Macs*                                          | |
| **Written in**       | Java, Kotlin                                                                                                         | |
| **License**          | [MOSAIC Extended](/download#overview)                                                                                | |
| **Website**          | [www.dcaiti.tu-berlin.de/research/simulation/phabmacs](https://www.dcaiti.tu-berlin.de/research/simulation/phabmacs) | |
|                      |                                                                                                                      | |

## Installation

{{% alert extended %}}
**PHABMACS** can be obtained as part of **[MOSAIC Extended](/download#overview)**.  
For further information on licenses, feel free to contact us via **[mosaic@fokus.fraunhofer.de](mailto:mosaic@fokus.fraunhofer.de)**.
{{% /alert %}}

1. Extract the **`phabmacs-federate-{{< version of="mosaic" >}}-bundle.zip`** archive into the `bin/fed/phabmacs` directory of your MOSAIC Extended installation. Create the directory, if not existing.
2. Navigate to `bin/fed/phabmacs` and execute the `activate_phabmacs.sh/bat` file. Enter your MOSAIC credentials.

## Overview

PHABMACS is designed to be a lightweight, developer-friendly, easy to modify tool to prototype cooperative driver assistance systems. 
Developers can use the simulated vehicles in PHABMACS first, before applying their code in real test vehicles.
The environment in PHABMACS allows for creation of arbitrary scenarios. 
Conveniently, such scenarios can be automatically generated from 
Open Street Map (OSM) map material using the street grid, buildings and natural elements like trees, grass, and water.
The 3D visualization is based on the [kool engine](https://github.com/kool-engine/kool) and comes with a puristic, yet appealing rendering style.

{{< figure src="../images/phabmacs_overview.png" title="Procedural generated environment model in PHABMACS." width="85%" >}}

{{< figure src="../images/phabmacs_overview_vehicle.png" title="Detailed assets for vehicles in PHABMACS." width="85%" >}}

## MOSAIC Scenario Integration

PHABMACS can be used in conjunction with MOSAIC the same way as other traffic simulators, such as SUMO.
The road network is generated on-the-fly from a given OSM file, which must be the same OSM file from which the MOSAIC scenario 
database was initially created.

### Basic Configuration

First of all, the simulator must be activated in the `federates` section of the `scenario_config.json` of the scenario:

```json
{
    "simulation": {
       ...
    },
    "federates": {
        "sumo": false,
        "phabmacs": true,
        "mapping": true, 
        "application": true
    }
}
```

Secondly, an OSM file must be placed inside the `phabmacs` directory of the scenario and referred in the `phabmacs_config.json`.
Furthermore, for testing the scenario, the `visualizer` should be enabled to enable the 3D visualization of PHABMACS.
A basic configuration should look like this:

```json
{
    "map": "ernst-reuter-platz_large.osm.gz",
    "stepSize": "100ms",
    "visualizer": true,
    "cameraPosition": {
        "latitude": 52.5122196,
        "longitude": 13.3148019
    }
}
```

### Vehicles and Routes

Vehicle routes are stored in the scenario-database, and vehicles are spawned using the `mapping_config.json`. 
Please refer to our general explanation of [MOSAIC Scenarios](/docs/scenarios) on how this is accomplished.

Additionally, the actual vehicle model needs to be configured in the phabmacs_config.json , where the vehicleProperties tag references a `vehicle.properties` file. 
This file outlines the key parameters such as geometry, dynamics, engine values, and more. 
It also specifies a 3D model for visualization using a `model.gz` file as asset, which are generated from Collada DAE files.
 
In the `vehicleTypes` section of the `phabmacs_config.json` vehicle types (which have been defined in the mapping configuration) are assigned with one of the available vehicle properties file:

```json
{
    ...
    "vehicleTypes": {
        "Car": {
            "vehicleProperties": "s-class.properties"
        },
        "SmallCar": {
            "vehicleProperties": "smart.properties"
        }
    }
}
```

PHABMACS currently supports model configurations including genuine parametrization and 3D models for the following types of vehicles:

| Vehicle Model         | Properties File      |
|-----------------------|----------------------|
| Smart fortwo          | `smart.properties`   |
| Mercedes-Benz C-Class | `c-class.properties` |
| Mercedes-Benz E-Class | `e-class.properties` |
| Mercedes-Benz B-Class | `b-class.properties` |
| Mercedes-Benz S-Class | `s-class.properties` |
| Police Car            | `police.properties`  |
| Schaeffler Mover      | `mover.properties`   |
| City Bus              | `bus.properties`     |

{{< figure src="../images/phabmacs_vehicles.png" title="Available vehicle models in PHABMACS." width="85%" >}}

### Vehicle Skills

The driving behavior of vehicles can be defined by declaring skills and features for each vehicle.
- A **skill** provides a vehicle with certain functionality, such as route following or strategic lane changing.
- A **feature** provides access to certain data a skill needs, for example, a list of surrounding entities (required by the lane change skill).

Each vehicle is always equipped with the route following and lane changing feature, and can optionally be equipped with the `ACC` skill, which keeps a safety distance towards the leading vehicle. 
This can be activated along with the vehicle types:

```json
{
    ...
    "vehicleTypes": {
        "Car": {
            "vehicleProperties": "s-class.properties",
            "skills": ["ACC"],
            "features": [],
            "sensors": []
        }
    }
}
```

{{% alert note %}}
Vehicles can be equipped with custom implementations for skills, features, and sensors. 
For that, a jar file with compiled code for these parts have to be placed inside the `bin/fed/phabmacs` directory, 
and the full qualified class name has to be used when adding skills or features to a vehicle type.
{{% /alert %}}

### LiDAR Sensor Configuration

One use-case of PHABMACS is to create synthetic sensor data, e.g., by utilizing a LiDAR sensor model. 
This can be achieved by adding LiDAR sensors to arbitrary vehicles by configuration in `phabmacs_config.json`.

First, we define which vehicle types should be equipped with LiDAR sensors. 
In the following example, all vehicles of type `Car` are equipped with a default lidar sensor.

```json
{
    ...
    "vehicleTypes": {
        "Car": {
            "vehicleProperties": "s-class.properties",
            "sensors": [ "LIDAR" ]
        }
    }
}
```

The LiDAR sensor can be configured by filling the `defaultLidarConfiguration` section in the `phabmacs_config.json`. 
The following example creates a lidar sensor at the front of the car, with a 120 degree field of view to the front, with 5 scanning rows (4 degree vertical field of 
view with 1 degree resolution) and 121 scanning columns (120 degree horizontal field of view with 1 degree resolution). 
The maximum range of the sensor is at 200m and the scan is done every 50 millisecond (20 Hz sampling rate).

```json
{
    ...
    "defaultLidarConfiguration": {
        "horizontalFieldOfView": 120,
        "verticalFieldOfView": 4.0,
        "horizontalResolution": 1.0,
        "verticalResolution": 1.0,
        "pitchOffset": 2.0,
        "range": 200.0,
        "samplingRate": 20.0,
        "position": {
            "x": 0.0,
            "y": 0.1,
            "z": -1.45
        }
    }
}
```

To see the result of the LiDAR scan, you can enable the visualization of LiDAR point clouds by setting the field `showLidar` to true.

{{< figure src="../images/phabmacs_lidar.png" title="Simulate LiDAR scanning of the environment and vehicles with PHABMACS." width="85%" >}}

## ScenarioSE

PHABMACS was initially developed as a standalone tool and comes with an own scenario specification in form of a programming API called **ScenarioSE**. 
This allows to programmatically create scenarios by spawning vehicles, adding skills and features, or add certain events. 
Vehicles could also be triggered depending on the position of other vehicles to orchestrate complex traffic situations.
It furthermore allows to integrate custom implementations of skills (vehicle control functions, such as vehicle following strategies), 
features (internal vehicle functions, such as lane changing), and/or custom sensor models. 

### Integration of ScenarioSE with MOSAIC

To integrate these kind of scenarios, the ScenarioSE code must be compiled into a jar file. 
The generated jar file must be placed in the `phabmacs` directory of the MOSAIC scenario. The fully qualified class name must then be referenced in the `phabmacs_config.json` file:

```json
{
    "stepSize": "100ms",
    "initialization": {
        "scenarioInitializer": "com.example.MyPhabmacsScenario",
        "waitForInitMessages": true
    },
    "visualizer": true
}
```

An example for a ScenarioSE definition is given in the following. 
This scenario loads a map, creates one route and spawns 100 vehicles which follow each other till the end:

```java
package com.example;

public class MyPhabmacsScenario implements ScenarioInitializer {
    
    @Override
    public ScenarioSE createScenarioSE() {
        ScenarioSE scenario = new ScenarioSE();

        // generate map from OSM data
        CoreMap map = CoreMap.loadOsmMap(scenario, "ernst-reuter-platz_large.osm.gz");
        scenario.setMapLayer(map);

        // create route from list of OSM nodes
        Route route = scenario.getMapLayer().buildRoute(
                new AbsoluteLocation(52.511688, 13.310000),
                new AbsoluteLocation(52.513334, 13.321019),
                new AbsoluteLocation(52.514046, 13.317432),
                new AbsoluteLocation(52.525800, 13.314159)
        );

        // spawn position will be at start of route on fourth lane
        Location spawnPos = RouteLocation.startLocation(route, 3);

        // configure camera position
        SimVisualizer vis = PhabmacsModules.getSimVisualizer();
        Vector3d lookAt = spawnPos.toAbsolute().getLocalPosition();
        Vector3d camPos = new Vector3d(lookAt).add(new Vector3d(-20.0, 20.0, 0.0));
        vis.setCameraConfig(camPos, lookAt);
                
        // configure spawner for vehicles
        RouteSpawner spawner = new RouteSpawner(scenario, route, spawnPos, 1)
                .setSpawnNVehicles(100)
                .setVehicleFactory(new DefaultVehicleFactory("s-class.properties"));

        // add a customizer which defines the vehicle behavior
        spawner.addVehicleCustomizer(vehicle -> {
            vehicle.getDriver().addSkill(new RouteFollowingSkill());            // ability to follow route
            vehicle.getDriver().addSkill(new LaneChangeSkill());                // ability to change lanes
            vehicle.getDriver().addSkill(new FrontVehicleFollowingSkill());     // ability to keep distance to leading vehicle
            vehicle.getFeatures().registerFeature(RouteProviderFeature.class, new StaticRouteProviderFeature(route));
            vehicle.getFeatures().registerFeature(SurroundingVehiclesFeature.class, new SurroundingVehiclesFeature());

            vehicle.getSimVehicle().setColor(Color.MAT_PINK);
        });

        // remove vehicles when route has been completed
        new EventDespawner(scenario, EndOfRouteEvent.class);

        return scenario;
    }
    
}
```

{{< figure src="../images/phabmacs_overview_2.png" title="Simulation scenario generated with the ScenarioSE definition above." width="85%" >}}