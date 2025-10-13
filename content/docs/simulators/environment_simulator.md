---
title: Environment Simulator
linktitle: Environment Simulator
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 100
menu:
  docs:
    parent: simulators
---

This ambassador can be configured with a configuration file. The specific path is `mosaic/scenarios/<scenarioName>/environment/environment_config.json`


```plaintext
└─ <scenario_name>
   └─ environment
      └─ environment_config.json ..................... Environment ambassador configuration file
```

## Installation

This simulator does not need to be installed. It is delivered as part of the Eclipse MOSAIC-installation package.

## Configuration

{{% alert tip %}}
Read the detailed documentation of the [Environment Configuration](/docs/mosaic_configuration/environment_config).  
{{% /alert %}}

The root node of the configuration is a list of environment events. 
Each event require the type of the event, its event value, an event area ("polygon", "rectangle", or "circle"), and the time window. 
The following example shows the configuration of an `OBSTACLE_ON_ROAD` event which is valid in the
designated area (rectangle) during the simulation time between 0 to 2000 seconds:

```json
{
    "events" : [
        {
            "type": "EVENT",
            "value": "OBSTACLE_ON_ROAD",
            "location": {
                "area": {
                    "type": "Rectangle",
                    "a": {
                        "latitude": 52.53654,
                        "longitude": 13.42116
                    },
                    "b": {
                        "latitude": 52.53435,
                        "longitude": 13.42366
                    }
                }
            },
            "time": {
                "start": "0 s",
                "end": "2000 s"
            }
        }
    ]
}
```

The configured events can be used within vehicle applications as soon as it enters such areas.
To achieve this, the `BasicSensorModule` must be acquired:

```java
@Override
public void onVehicleUpdated(VehicleData previousVehicleData, VehicleData updatedVehicleData) {

    getOs().getBasicSensorModule().getSensorValue(Sensor.EVENT).ifPresent(event -> {
        if (event == EnvironmentEventCause.OBSTACLE_ON_ROAD) {
            // do something
        }
    });
}
```

## Examples

Configure a traction hazard in a circular area which is valid during the whole time of the simulation:

```json
{
    "events" : [
        {
            "type": "TRACTION_HAZARD",
            "value": "ICE",
            "location": {
                "area": {
                    "type": "Circle",
                    "center": {
                        "latitude": 52.53654,
                        "longitude": 13.42116
                    },
                    "radius": 30
                }
            }
        }
    ]
}
```

```java
getOs().getBasicSensorModule().getSensorValue(Sensor.TRACTION_HAZARD).ifPresent(hazard -> {
    if (hazard == TractionHazard.ICE) {
      // do something
    }
});
```
---

Configure a specific road condition value (e.g., as IRI) which is valid on a specific road:

```json
{
    "events" : [
        {
            "type": "ROAD_SURFACE_CONDITION",
            "value": 5,
            "location": {
                "connectionId": "8972_5082_1002"
            }
        }
    ]
}
```

```java
getOs().getBasicSensorModule().getSensorValue(Sensor.ROAD_SURFACE_CONDITION).ifPresent(iri -> {
    if (iri > 2.0) {
      // do something
    }
});
```
---

Configure a global windy event which only occurs for a short period of time:

```json
{
    "events" : [
        {
            "type": "WIND",
            "value": { 
                "speed": 30,
                "direction": 45
            },
            "time": {
                "start": "300 s",
                "end": "360 s"
            }
        }
    ]
}
```

```java
getOs().getBasicSensorModule().getSensorValue(Sensor.WIND).ifPresent(wind -> {
    if (wind.speed > 20.0) {
      // do something
    }
});
```