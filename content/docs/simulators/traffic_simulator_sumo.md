---
title: Eclipse SUMO - Simulation of Urban MObility
linktitle: Traffic - Eclipse SUMO
toc: true
type: docs
draft: false
weight: 20
menu:
  docs:
    parent: simulators
---
**Eclipse SUMO** is a highly portable, microscopic and continuous road traffic
simulation tool. It is designed to handle large road networks faster than real-time and simulates each vehicle
 individually.

|                          |                                                    | |
|--------------------------|----------------------------------------------------|-|
| **Operating System**     | GNU/Linux, macOS, Microsoft Windows                | |
| **Written in**           | C++                                                | |
| **License**              | EPL-2.0                                            | |
| **Website**              | [https://www.eclipse.dev/sumo/](https://www.eclipse.dev/sumo/) | |
| **Supported version(s)** | Recommended version:<br>Full support:<br>Limited support: | {{< version of="sumo" >}}<br>{{< version of="sumo_full_support" >}}<br>{{< version of="sumo_limited_support">}} |
|                          |                                                    | |

## Installation

{{< button icon="external-link-alt" title="Download Eclipse SUMO" link="https://sumo.dlr.de/wiki/Downloads" >}}

Download the SUMO binary bundle or installer from the SUMO website. Linux users may build SUMO from the source code.
Depending on the distribution, it is even possible to install SUMO as package from a software repository.
MacOS users can be required to install XQuartz.
Please refer to the SUMO Wiki for further information.

{{% alert note %}}
In order to run SUMO with Eclipse MOSAIC you need to make the SUMO binaries available system-wide by adding the SUMO
binary folder to your `PATH` environment variable. For many tools SUMO requires the environment variable `SUMO_HOME`
to be set to the installation directory of SUMO.
{{% /alert %}}

On macOS you can find the SUMO install directory using:
```bash
pkgutil --pkgs
pkgutil --pkg-info org.eclipse.sumo.framework
```
## Configuration

The SUMO ambassador can be configured with a configuration file. The specific path is `<scenarioName>/sumo/sumo_config.json`.
If no such file exists, the following default configuration options are used:

```json
{ 
    "updateInterval": 1000, 
    "sumoConfigurationFile": "<scenarioName>.sumo.cfg", 
    "exitOnInsertionError": true, 
    "additionalSumoParameters": "--time-to-teleport 0  --seed 100000", 
    "subscriptions": [ "roadposition", "signals", "emissions" ],
    "subscribeToAllVehicles": true
}
```

{{% alert tip %}}
Read the detailed documentation of the [SUMO Configuration](/docs/mosaic_configuration/sumo_config).  
{{% /alert %}}

Next to `sumo_config.json`, the following configuration files are required for every SUMO simulation scenario:

```plaintext
└─ <scenario_name>
   └─ sumo
      ├─ <scenarioName>.net.xml .............. SUMO Network file
      ├─ <scenarioName>.sumocfg .............. SUMO configuration file
      └─ sumo_config.json .................... Ambassador configuraition file]
```

The SUMO configuration consists of sumo specific config files and the sumo-ambassador configuration
file. The main configuration file name must end with the suffix *.sumocfg, which needs to refer to the network.
The network file is mandatory and can be generated with the `scenario-convert` tool provided with Eclipse MOSAIC.

{{% alert warning %}}
When you are coming from SUMO you might notice the missing route file (`*.rou.xml`). This is because with Eclipse MOSAIC, 
the traffic definition (definition of vehicles, flows, vehicle types) is usually part of the Mapping configuration file. Routes
usually are defined in the Application database. You can however add route files to your scenario and mosaic will handle
all vehicles in coherence.
{{% /alert %}}

Vehicle related parameters, such as acceleration, maximum speed, and the like, are configured via the Mapping configuration file. However,
some SUMO specific parameters, like the car following model can only be configured in the `sumo_config.json`. For example, if you have
configured a vehicle type called `MyVehicle` in the Mapping configuration, you can set specific parameters for this type as following:

```json
{
    ...,
    "additionalVehicleTypeParameters": {
        "MyVehicle": {
            "carFollowModel": "IDM",
            "lcKeepRight": "10"
        },
        ...
    }
}
```

{{% alert note %}}
All parameters have to be specified as Strings.
{{% /alert %}}

Further information about SUMO and its configuration can be found in the official SUMO wiki.

## Using the SUMO GUI with Eclipse MOSAIC

It is also possible to use the graphical interface of SUMO in order to visualize and interact with the simulation.
To
achieve this, Eclipse MOSAIC can be configured to start the GUI process of SUMO as the federate rather than the
command
line interface.

In order to use the SUMO GUI the file `<mosaic>/etc/runtime.json` needs to be edited.
Here, the entry
`org.eclipse.mosaic.fed.sumo.ambassador.SumoAmbassador` must be
replaced with
`org.eclipse.mosaic.fed.sumo.ambassador.SumoGuiAmbassador`.

{{% alert note %}}
Keep in mind to launch Eclipse MOSAIC with the argument `-w 0` in order to disable the watchdog timer.
Otherwise, it
would shut down Eclipse MOSAIC if the simulation is paused in the SUMO GUI.
{{% /alert %}}

## Using LibSumo with Eclipse MOSAIC

The coupling between MOSAIC and SUMO is implemented in two ways. The default implementation uses
a socket-based API provided by SUMO, short TraCI. This interface has been well-established over the recent years
and integrates very well with MOSAIC. The main disadvantage of this method is, that it uses a socket to
transmit all the simulation data from SUMO to MOSAIC, even if SUMO runs on the very same machine. This
may result in a bottleneck and slows down the simulation especially for large-scale scenarios.

To overcome this, there is a second method to couple SUMO and MOSAIC with each other. This integrates
SUMO as a dynamic linked library into MOSAIC, which results in a much faster data exchange. In order
to get this running, you need to follow these steps:
* Make sure `SUMO_HOME` environment variable is set correctly to your SUMO installation.
* Check, if there's a `bin/libsumojni.dll` or `bin/liblibsumojni.so` file in your SUMO installation directory. If not, you might
  need to install the "extras" release bundle of SUMO (Windows), or you need to re-build SUMO from sources and activate
  the build of libsumo (see https://sumo.dlr.de/docs/Libsumo.html#building_it).
* Only the recent SUMO version ({{< version of="sumo" >}}) is supported.

If these pre-requisites are met, you can activate this feature editing the file `<mosaic>/etc/runtime.json`.
Here, the entry
`org.eclipse.mosaic.fed.sumo.ambassador.SumoAmbassador` must be
replaced with
`org.eclipse.mosaic.fed.sumo.ambassador.LibSumoAmbassador`.

{{% alert note %}}
Please be aware that this feature is still experimental. Also, some SUMO commands are not yet supported
(retrieving leader information, partly missing information about traffic lights and induction loops).
{{% /alert %}}

## Adding Vehicles
The `SumoAmbassador` handles vehicles added via Mapping (*mapping vehicles*) and via SUMO route files (*sumo vehicles*).
There are however some caveats:
* *Mapping vehicles* can drive on routes specified in route files, however *sumo vehicles* can't drive on routes specified in the scenario
database
* In order to configure an application mapping for *sumo vehicles*, you can either use the `prototypes` sections for single type mapping, or the `typeDistribution` section for choosing between multiple mappings by weight. It is currently not possible to map applications on specific vehicles, unless they have a distinctive vehicle type name.
* IDs of your *sumo vehicles* will be replaced on MOSAIC side with internal IDs starting with `veh_` prefix. Keep this in mind when your applications create log files or during debugging.
* The vehicle types defined in Mapping and defined in route files can't share the same names

This duality of adding vehicles has some powerful use cases. For example, you can use an existing SUMO scenario and add your own
traffic via MOSAIC and equip all vehicles with applications.

## Deep dive: Route Files / Additional Files

In SUMO the route files (`<..>.rou.xml`) fulfill three main purposes supported by Eclipse MOSAIC:
1. Define vehicle types.
2. Define routes for vehicles.
3. Define vehicle departures to spawn vehicles with defined types (1.) on defined routes (2.)

These definitions can also be done in additional files (`<...>.add.xml`).

Route and vehicle departure definitions can also be handled by SUMO's **Tr**affic **C**ontrol **I**nterface (TraCI), which is also
the way Eclipse MOSAIC adds them to the simulation. This has the advantage that it can be done at runtime leading to a smaller overhead
before simulation start. Vehicle types however have to be defined in a route file, or an additional file before simulation start,
additional files have the advantage, that they are loaded before route files, which is helpful for our use case.
We write a new additional file (`mosaic_types.add.xml`), which contains types specified in Mapping and merges them
with the aforementioned `additionalVehicleTypeParameters` (Note: these are not validated in any form). The image below shows
a schematic view of how the configuration files and RTI components interact with each other.
{{< figure src="../images/sumo_route_files.svg" title="Schematic overview of vehicle type handling in Eclipse MOSAIC" >}}

## Access SUMO TraCI From Applications

If SUMO is used as a traffic simulator and a special functionality is required, the `sendSumoTraciRequest`
function in the `OperatingSystem` can be used.

The function expects a string (a unique identifier which will be assigned to the response) and a byte array
(representing the complete Traffic Control Interface (TraCI) request including the header). The message
identifier can be an empty string.

In all cases the command will trigger a response. The application can receive the response from the
method `onSumoTraciResult`. This method will receive a `SumoTraciResult`
object. This response contains the specified identifier. The application must handle the
identification process of the response itself.

{{% alert note %}}
Be careful when using this interface and the TraCI commands. The
commands are delivered to TraCI without any prior checks.
{{% /alert %}}

{{% alert note %}}
You can find the example application SumoTraciInteractionApp
in the additional examples bundle on the {{< target-blank "DCAITI website" "https://www.dcaiti.tu-berlin.de/research/simulation/download/" >}}.
{{% /alert %}}
