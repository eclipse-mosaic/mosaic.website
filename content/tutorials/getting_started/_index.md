---
title: Getting Started with Eclipse MOSAIC
categories:
   - Tutorial
linktitle: tutorial_getting_started
toc: false
type: tutorials
draft: false
pagination_next: barnim_simulation_basics
---

Eclipse MOSAIC can be downloaded as an executable bundle (see [our download section](/download)). This bundle
contains almost everything you need to execute simulation scenarios with Eclipse MOSAIC.

{{% alert learning_objectives %}}
With this tutorial you learn...  
* Download and Install Eclipse MOSAIC
* Running and configuring a simulation scenario
* Gather simulation results
{{% /alert %}}

## Requirements

{{% alert note %}}
Some prerequisites are required on your machine. Eclipse MOSAIC is a software based on JAVA, and therefore, a 
JAVA Runtime Environment or Development Kit is required:
- **JAVA Runtime Environment** (version {{< version of="java" >}} minimum and recommended), e.g. [Eclipse Temurin OpenJDK](https://adoptium.net/de/temurin/releases?version={{< version of="java_recommended" >}})
{{% /alert %}}

{{% alert note %}}
In addition, the pre-bundled simulation scenarios are configured to use certain simulators. For traffic simulation, the open-source 
traffic-simulator SUMO is required:
- **Eclipse SUMO** (latest version {{< version of="sumo" >}} recommended), [https://www.eclipse.dev/sumo](https://www.eclipse.dev/sumo).
{{% /alert %}}

Additional simulators are not required by the pre-bundled scenarios. However, if you plan to simulate communication scenarios
in high detail, the simulators [ns-3](/docs/simulators/network_simulator_ns3) or 
[OMNeT++](/docs/simulators/network_simulator_omnetpp) are recommended. For that, additional 
information can be found in our **[documentation](/docs/getting_started)**.

## Download and install

1. **Download the `eclipse-mosaic-{{< version of="mosaic" >}}.zip` bundle from [our download section](/download).** 
2. Extract the package to an arbitrary path.

## Run Eclipse MOSAIC

Once you've installed everything, you should be able to run your first simulation. For this, switch to your terminal application and type:

On Unix/Linux/macOS:
```bash
./mosaic.sh -s Barnim -v
```

On Windows:

```dos
mosaic.bat -s Barnim -v
```

This will execute the [Barnim](/tutorials/barnim_simulation_basics) scenario. During the simulation, vehicles drive along the
road and exchange V2X messages between each other. With the `-v` parameter, a 2D visualization opens in your default browser. The whole
procedure should look like this:

{{< figure src="../../docs/getting_started/images/mosaic-barnim.gif" title="Barnim scenario executed with Eclipse MOSAIC" numbered="true" >}}

To slow down the simulation (e.g. to get a slower visualization), you can use the realtime brake parameter `-b 5`. 
Further information about the start parameters of MOSAIC can be found in our [documentation](/docs/getting_started/run_mosaic).

## Gather simulation results

Eclipse MOSAIC generates log files for each simulation run. Log files are generated for the ambassadors of each coupled federate respectively 
simulator and for the RTI itself. The log files are stored in the folder `<mosaic-root>/logs/log-<timestamp>`. For each simulation run a new folder is created. 

```plaintext
└─ log-<timestamp>
   ├─ apps
   |  └─ <unitType>_<unitId> ................. Detailed application specific logs for each unit
   |      ├─ OperatingSystem.log ............. Detailed operating system logs for the unit
   |      └─ ExampleApp.log .................. Detailed application specific logs for each application.
   ├─ Application.log  ....................... Information about the application ambassador
   ├─ Communication.log ...................... (Ad-hoc) network simulation ambassador log
   ├─ CommunicationDetails.log ............... Detailed output of network simulator (ns-3 or OMNeT++)
   ├─ Mapping.log ............................ Mapping configuration logs
   ├─ MOSAIC.log ............................. General information, e.g. startup sequence information
   ├─ Traffic.log ............................ Traffic simulation log (SUMO or others)
   └─ output.csv ............................. Recorded data of the integrated File Output Generator
```

Further information on log files can be found {{< link title="here" href="/docs/getting_started/results/">}}.

For uniformly formatted or visually prepared results, Eclipse MOSAIC offers different 
Visualizers and Output Generators. For example, the `FileOutputGenerator` generates a `outputs.csv` file with detailed outputs of e.g. 
vehicle positions, speeds, or message exchanges:

```plaintext
CELL_CONFIGURATION;6000000000;veh_0;true;7200000000;1400000000
V2X_MESSAGE_TRANSMISSION;6000000000;DENM;3;rsu_0;52.65027;13.545;0.0;CELL_GEOCAST;/255.255.255.255;null
VEHICLE_UPDATES;7000000000;veh_0;35.501624617716296;186.33236029307432;52.655993308955196;13.569065826100868;0.0;35.501624617716296;-0.6083753822837039;0.0;false;1;4067968_28830219_3290027832_2450938914;0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;false;false;false
VEHICLE_REGISTRATION;7000000000;veh_1;ElectricVehicle;null;Unequipped;5.0;2.5;70.0;2.6;4.5;0.5;1.0;1.0;0.0;1;1;0.0
VEHICLE_UPDATES;8000000000;veh_0;34.978651295430026;186.33236029306624;52.65568017869267;13.569019012494635;0.0;70.48027591314633;-0.5229733222862691;0.0;false;1;4067968_28830219_3290027832_2450938914;0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;0.0;false;false;false
V2X_MESSAGE_TRANSMISSION;8000000000;DENM;4;rsu_0;52.65027;13.545;0.0;CELL_GEOCAST;/255.255.255.255;null
```

More information for output generation can be found in our [documentation](/docs/visualization/filevis).


## Scenario Configuration

The configuration of a simulation scenario consists of a main configuration file `scenario_config.json` and a configuration file for each simulator. A minimal simulation scenario consists of the following  files:

```plaintext
└─ <scenarioName>
   ├─ application
   |  └─ <scenarioName>.db................ Scenario database file
   ├─ mapping
   |  └─ mapping_config.json ............. Mapping configuration file
   ├─ sumo
   |  └─ <scenarioName>.net.xml .......... SUMO network file
   |  └─ <scenarioName>.sumocfg .......... SUMO configuration file
   └─ scenario_config.json ............... Basic configuration of the simulation scenario
``` 

The scenario database contains a model of the road network (the map), and the mapping configuration
defines all entities present in the scenario, such as vehicles, servers, or road side units. Additionally, necessary
configuration files for the traffic simulator SUMO belong to the scenario folder. Depending on the scope and
complexity of the simulation scenario, additional configuration files are required. A detailed description of 
the structure and configuration possibilities of scenarios and simulators can be found in 
our  [documentation](/docs/scenarios).

The main configuration contains the list of simulators to use for the scenario. This way, simulators can be easily added or removed from the simulation. Scenario duration can also be configured here. Parameters like `projection` or `randomSeed` do not need to be configured manually and are generated upon scenario creation.

```json
{
    "simulation": {
        "id": "Barnim",
        "duration": "1000s",
        "randomSeed": 268965854,
        "projection": {
            "centerCoordinates": {
                "latitude": 52.511289,
                "longitude": 13.3167457
            },
            "cartesianOffset": {
                "x": -385769.05,
                "y": -5819239.29
            }
        }
    },
    "federates": {
        "application": true,
        "cell": false,
        "environment": true,
        "sns": true,
        "ns3": false,
        "omnetpp": false,
        "output": true,
        "sumo": true
    }
}
```
