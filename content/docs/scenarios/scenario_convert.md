---
title: Scenario-Convert Tool
linktitle: Scenario-Convert Tool
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 20
menu:
  docs:
    parent: scenarios
---

{{% alert note %}}
**Scenario-Convert** does not belong to the open-source part of Eclipse MOSAIC and must be obtained separately.

Yet, you can use Scenario-Convert **for free** to generate scenarios which are executable with Eclipse MOSAIC. **[Get it here](https://www.dcaiti.tu-berlin.de/research/simulation/download/).**
{{% /alert %}}

{{%alert info %}}

The command line interface of **Scenario-Convert** received a major update with MOSAIC 25.1.
Please follow the `--help` command, or use this page to learn how to use the new CLI.

{{% /alert %}}

{{< button icon="download" type="primary" link="https://www.dcaiti.tu-berlin.de/research/simulation/download/" title="Download scenario-convert from DCAITI mirror" >}}

1. **Download the `scenario-convert-{{< version of="mosaic" >}}.zip/tar.gz` bundle from the mirror above.**
2. Extract the package to an arbitrary path. It is recommended to extract the package in the very same directory as
   MOSAIC (`<mosaic-root>`).
3. Use the call `scenario-convert.bat/sh` on the command line to execute the tool.

## Usage of scenario-convert

{{% alert note %}}
The [Create a new Scenario Tutorial](/tutorials/create_new_scenario) provides a detailed introduction to the scenario-convert tool.
{{% /alert %}}

**Scenario-Convert** is a tool for generating and modifying MOSAIC scenarios.
In the following we first explain its operation commands and general
arguments. 
Secondly, we list various use cases of creating or altering scenarios, supported by several examples.

For a full list of commands and arguments, please follow the `--help` command. 

## Operation Commands

Scenario-Convert offers various commands, which are followed by various additional arguments and parameters to solve specific use-cases to prepare
your MOSAIC scenario. The use-cases are described further below. The following commands exist:

| <div style="width:110pt">Command</div> | Description                                                                                                                                       |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| `scenario create`                      | Creates directory structure for a MOSAIC scenario based on an OSM file, a SUMO scenario, or an existing scenario database.                        |
| &nbsp;                                 |                                                                                                                                                   |
| `database create`                      | Creates a scenario database based on an OSM file or a SUMO network file.                                                                          |
| `database import`                      | Adds additional information to the scenario database, such as elevation data or buildings, or replaces the road network from a SUMO network file. |
| `database export`                      | Exports the scenario database to a SUMO network file or GeoJSON files.                                                                            |
| `database update`                      | Updates the database schema of an existing scenario database from an old version to the current version.                                          |
| &nbsp;                                 |                                                                                                                                                   |
| `route generate`                       | Generates route(s) from one point to another.                                                                                                     |
| `route import`                         | Imports routes from a SUMO route file to an existing scenario database.                                                                           |
| `route export`                         | Exports routes from the scenario database to a SUMO route file or GeoJSON file.                                                                   |
| `route list`                           | Lists all routes which are stored in the scenario database.                                                                                       |
| `route clear`                          | Removes all routes from the scenario database.                                                                                                    |
| &nbsp;                                 |                                                                                                                                                   |
| `mapping create`                       | Creates a mapping configuration based on the given SUMO route file.                                                                               |
| `mapping export`                       | Exports vehicle types and spawners from a given mapping file to a SUMO route file.                                                                |
| &nbsp;                                 |                                                                                                                                                   |
| `regions create`                       | Creates a regions configuration for the MOSAIC Cell simulator based on GeoJSON.                                                                   |
| `regions configure`                    | Re-configure network properties of a MOSAIC Cell configuration.                                                                                   |
| `regions export`                       | Exports a MOSAIC Cell regions configuration to a GeoJSON file.                                                                                    |


## General Hints

Each command usually requires an argument which points to a file. 
This file serves as the input for the command and the supported file type depends on the command. 
Some commands support multiple file types, which are internally identified by their filename extensions.

The tool provides a help command (`-h` or `--help`) which can be used to generate a general help screen,
(`scenario-convert.bat --help`), or specifically for a command (e.g., `scenario-convert database create --help`).

## Use case overview

In the following, we explain the usage of Scenario-convert in the context of typical use-cases when it comes to scenario creation:

|                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| :arrow_forward: [Create MOSAIC scenario from OSM file](scenario_convert.md#-create-mosaic-scenario-from-osm-file)                                   |
| :arrow_forward: [Create MOSAIC scenario from SUMO file](scenario_convert.md#-create-mosaic-scenario-from-sumo-file)                                 |
| :arrow_forward: [Create/Import routes for existing MOSAIC scenario](scenario_convert.md#-createimport-routes-for-existing-mosaic-scenario)          |
| :arrow_forward: [Import height information to existing MOSAIC scenario](scenario_convert.md#-import-height-information-to-existing-mosaic-scenario) |
| :arrow_forward: [Export database or routes to GeoJson](scenario_convert.md#-export-database-or-routes-to-geojson-files)                             |
| :arrow_forward: [Create and Export MOSAIC Cell Region Configurations](scenario_convert.md#-create-mosaic-cell-regions-configuration-from-geojson)   |

---

### **:arrow_forward: Create MOSAIC scenario from OSM file**

There are two ways to create a MOSAIC scenario from an OSM file using scenario-convert.

**1. Using a single call with `scenario create`:**

The easiest method to create a MOSAIC scenario from OSM data is to use scenario-convert with the command `scenario create`. 
One only needs to specify the path to the input OSM file. 
The OSM file will first be used to create a scenario database containing road network information.
Afterward, the MOSAIC scenario folder structure and SUMO connection, edge, network, node, route, and config files are created.
You can also use additional arguments alongside this, for example `--generate-routes`. 
With this argument, routes are generated randomly and added to the scenario database before the scenario is exported to MOSAIC scenario files.

The call for the described example would look like this: <br>

```shell
scenario-convert.bat scenario create "/path/to/map.osm" --generate-routes 
```

**2. Using two separate calls with the option `database create` and `scenario create` subsequently:**

This works very similarly to the first approach, but gives you more control with handling the database-file. 
The first call creates the database from the OSM file. 
As a result, there is just the database file that contains all relevant road network information. 
It is possible to now alter the database, for example by generating routes or including elevation information. 
How to do both of these is described below.
After all changes to the database are completed, you can now call scenario-convert a second time using the second argument and create a
complete MOSAIC scenario structure and SUMO connection, edge, network, node, route, and config files from the database.

Example calls for this approach would look like this: <br>
**First call:**

````shell
scenario-convert.bat database create "/path/to/map.osm" -s "myScenario"
````
This will create a `myScenario.db` file in the same directory of the input file `map.osm`.
The `-s` argument here is optional. If not specified, the database will be named the same as the OSM file. <br>

**Second call:**

````shell
scenario-convert.bat scenario create "/path/to/myScenario.db" 
````

**Additional arguments usable with `database create` argument when importing OSM files**

| Argument                        | Explanation                                                                                                                                                                                                                                        |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--skip-buildings`              | Skips the import of building information into the database                                                                                                                                                                                         |
| `--skip-graph-cleanup`          | Turns off the removal of unconnected parts from the main traffic network graph . Since several components of MOSAIC require one main graph without disconnected ways and nodes, this option should be used only if the cleanup procedure is faulty |
| `--skip-reduce-ways`            | Skips automatic removal of irrelevant ways of the OSM file                                                                                                                                                                                         |
| `--skip-turn-restrictions`      | Ignore all defined turn restrictions on OSM import                                                                                                                                                                                                 |
| `-s / --scenario-name <String>` | Name to be used for generating the database file.                                                                                                                                                                                                  |

---

### **:arrow_forward: Create MOSAIC scenario from an existing SUMO scenario**

If you want to convert an already existing SUMO scenario to a usable MOSAIC scenario, you can do that in a similar manner using the following command.

**Using a single call with `scenario create`:**

The concept of this is the same as for creating a MOSAIC scenario from an OSM file. 
Simply specify the path to the `*.sumocfg` file of your existing SUMO scenario. 
Scenario-convert will then wrap the existing SUMO scenario in a MOSAIC scenario by moving SUMO files to the `sumo` directory of the resulting MOSAIC scenario. 
Furthermore, all other relevant MOSAIC files are created and the projection information is stored in the `scenario_config.json` file.

This example's call would look like this:

```shell
scenario-convert.bat scenario create "/path/to/sumo.sumocfg"
```
If the provided scenario has no projection information stored in its network file, then the parameter `--import-zone` is required.
The value of this option defines the UTM zone your scenario lies in, e.g., `33u` for the east of Germany.

**Additional Arguments with `scenario create` when importing SUMO scenario:**

| Argument                        | Explanation                                                                                  |
|---------------------------------|----------------------------------------------------------------------------------------------|
| `-i / --import-zone <String>`   | UTM zone of location for projecting coords in default format (e.g. 33u)                      |
| `-s / --scenario-name <String>` | Specifies the name of the scenario. If not given, the base name of the input file is chosen. |

---

### **:arrow_forward: Create/Import routes for existing MOSAIC scenario**

Often you have an existing database representing your road network but no routes for vehicles yet. 
Routes can be either created from scratch or imported from SUMO route files using scenario-convert.

**1. Import a SUMO route file into a database:**

Importing a route file is possible with just a single call of scenario-convert. 
This call reads the routes in the file, saves each route's id and edges, and creates objects in the database for them.

````shell
scenario-convert.bat route import "/path/to/database.db" "/path/to/routefile.rou.xml"
````

**2. Creating new routes:**

Creating new routes with scenario-convert can be done in two ways. Either by creating random routes through the network using only
`--generate-routes` or by additionally specifying start and end point of the route you want to create. Random routes are always created
based on road end points in your network, meaning they always range from one edge of the map to another or from dead end to dead end.
You can also specify start and end point of a route, either in geo coordinates or through node IDs.

The following examples showcase multiple approaches to generating routes using scenario-convert.

**Generate X random end-to-end routes using `-g / --generate-routes`:**

If you want to generate many routes fast, this option is a good possibility to do that.

````shell
scenario-convert.bat route generate "/path/to/database.db" --max 2
````

You can cap the number of routes generated by passing the argument `--max X`, where X is a positive integer.
Scenario-convert then generates a maximum of X routes for each origin/destination pair. 
Per default, only 1 route is calculated for each origin/destination pair.

**Calculate a single route from point A to point B:**

In contrast to the example above, if a start and end point is given, scenario-convert only calculates a single route between these points
if `--max` is not given.

````shell
scenario-convert.bat route generate "/path/to/database.db" --from 52.526371,13.314075 --to 52.512879,13.320099
````

The points can be given in geo coordinates or as node IDs.
If given in geo coordinates, make sure the coordinates are within the area of your scenario network.

**Calculate X alternative routes from point A to point B:**

If start and end point are given and `--max X` is passed, where X is a positive integer, scenario-convert calculates
multiple routes between the points, with a maximum of X alternatives. It is not guaranteed that X routes are calculated.

````shell
scenario-convert.bat route generate "/path/to/database.db" --from 52.526371,13.314075 --to 52.512879,13.320099 --max 4
````

[//]: # (**Calculate all possible routes between a list of points:**)

[//]: # ()
[//]: # (If you want to calculate routes between multiple points you can pass a list of node IDs with the argument `--route-matrix`.)

[//]: # (This will generate all possible routes between all points in this list.)

[//]: # ()
[//]: # (````shell)

[//]: # (scenario-convert.bat --generate-routes -d '/path/to/database.db' --route-matrix '12345,98765,13579' )

[//]: # (````)

**3. View all routes in a database:**

If you have already created routes and want to view them without using external tools, you can use the argument `--list-routes`.
In combination with `-d` to set the path to the database you want to view, this outputs a summary of all routes in the database consisting
of route ID, start node ID, end node ID, and route length in meter.

**View routes in a database by using the following call:**

````shell
scenario-convert.bat route list "/path/to/database.db"
````

**Remove all routes in a database by using the following call:**

````shell
scenario-convert.bat route clear "/path/to/database.db"
````

---

### **:arrow_forward: Import height information to existing MOSAIC scenario**

Importing height information into the database can be done with the command `database import` along with the `--elevation` option. 
It optionally accepts an SRTM file, whose elevation data is then written to the nodes of an existing database.

**An example call would look like this:**

````shell
scenario-convert.bat database import "/path/to/database.db" --elevation "/path/to/srtm.geotiff"
````

If no path to an elevation file is given, elevation data is downloaded from a server.

````shell
scenario-convert.bat database import "/path/to/database.db" --elevation
````

---

### **:arrow_forward: Export database or routes to GeoJSON files**

Sometimes you might want to export the contents of your database into a GeoJSON format. For example for visualizations of your road network
using QGIS.

**Use this scenario convert call for this purpose:**

````shell
scenario-convert.bat database export "/path/to/database.db" --geojson
````

This will export the database content into several .geojson files with the same name as the database.

````shell
scenario-convert.bat route export "/path/to/database.db" --geojson
````

This will additionally export all routes into a .geojson file.

---

### **:arrow_forward: Create MOSAIC Cell Regions Configuration from GeoJSON**

The MOSAIC Cell Simulator requires a configuration file which contains locations, areas, and network properties of individual cell regions.
This file can be hard to write manually, therefore, Scenario Convert some helper methods. 
One of these, allows to create a regions configuration from polygons defined in a GeoJSON file.
 
**Create a Cell regions configuration with the help of GeoJSON:**

As a start, you can use [geojson.io](https://geojson.io/#map=11.66/52.493/13.4134) to create several polygons using a map, which would serve as the cellular regions.
Once finished, you can save and download the result as a GeoJSON file.
With Scenario Convert, you can use the following command to create a regions configuration based on the generated GeoJSON file:

```shell
scenario-convert.bat regions create "/path/to/regions.geojson"
```

This will generate a `regions.json` in the same directory as the input file, which can be loaded as a regions configuration for the MOSAIC Cell Simulator.

**Adjust the network properties for all Cell regions:**

In the previous call, the network properties for each region are filled with default values.
You can adjust these properties by hand, or use Scenario Convert to change certain properties for all regions in one single step:

```shell
scenario-convert.bat regions configure "/path/to/regions.json" -f --set-random-delay 30ms --delay-min 5ms --set-uplink-capacity 100Mbps --set-downlink-capacity 1Gbps --set-packet-loss 0.01
```

This call does various changes to the Regions configuration for each region:
* Sets the delay for all communication links to a random gamma delay with an expected delay of 30 milliseconds.
* Sets the total maximum capacity of the cell region for the uplink to 100Mb per second, and for the downlink to 1Gb per second.
* Sets the packet loss probability for all communication attempts to 1%. 

**Export the cell regions file for visualization or editing to GeoJSON:**

Furthermore, you can use Scenario Convert to export an existing Cell regions configuration to a GeoJSON file.

```shell
scenario-convert.bat regions export "/path/to/regions.json"
```

This will generate a `regions.geojson` in the same directory as the input file.
All network properties are appended to the properties section of each GeoJSON feature.
Therefore, after altering the file, you can import the file again to recreate the Cell regions configuration file:

```shell
scenario-convert.bat regions create "/path/to/regions.geojson" -f
```