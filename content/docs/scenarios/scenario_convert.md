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
**scenario-convert** does not belong do the open-source part of Eclipse MOSAIC and must be obtained separately.

Yet, you can use scenario-convert **for free** to generate scenarios which are executable with Eclipse MOSAIC. **[Get it here](https://www.dcaiti.tu-berlin.de/research/simulation/download/).**
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

[//]: # (The following listing shows an overview for the usage of scenario-convert:)

[//]: # ()

[//]: # (```)

[//]: # ({{< include_file path="files/ScenarioConvertFunctions.txt" >}})

[//]: # (```)
Scenario-convert can be used for a lot of different operations. In the following we first explain its operation modes, then general
arguments for this tool, and finally we list all of its use cases, provide examples and explain arguments used to achieve the results.

## Operation Modes

Scenario-convert offers various operation modes, which are followed by various additional parameters to solve specific use-cases to prepare
your MOSAIC scenario. The use-cases are described further below. The following operation modes exist:

| Operation Mode      | Description                                                                                                                                             |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--db2sumo`         | Exports the scenario database to a SUMO network file.                                                                                                   |
| `--db2mosaic`       | Creates directory structure for a MOSAIC scenario based on the database .                                                                               |
| `--db2geojson`      | Exports the scenario database into GeoJSON format.                                                                                                      |
| `--osm2db`          | Converts an OpenStreetMap file to a new scenario database.                                                                                              |
| `--osm2sumo`        | Combination of `--osm2db` and `--db2sumo`.                                                                                                              |
| `--osm2mosaic`      | Combination of `--osm2db` and `--db2mosaic`.                                                                                                            |
| `--srtm2db`         | Imports an SRTM file and writes elevation data to nodes.                                                                                                |
| `--sumo2db`         | Converts a SUMO Network to a new scenario database, or imports routes to an existing scenario.                                                          |
| `--sumo2mosaic`     | Combination of `--sumo2db` and `--db2mosaic`. When provided with a `*.sumocfg` file, this mode converts an existing SUMO scenario to a MOSAIC scenario. |
|                     |                                                                                                                                                         |
| `--generate-routes` | Generates route(s) from one point to another. Can be used alone with existing databases, or be used in combination with other operation modes.          |

## General Arguments

The following arguments are relevant for all operation modes and use cases:

| Argument                     | Description                                                                                                                                                                   |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-h`, `--help`               | Prints the help screen of the scenario-convert tool                                                                                                                           |
| `-d`, `--database <PATH> `   | The path to the scenario database                                                                                                                                             |
| `-i`, `--input <PATH>`       | The path to the input file. The file type depends on the use case, see the sections below                                                                                     |
| `-f`, `--force`              | Force overwrite of existing files instead of incrementing file names                                                                                                          |
| `-c`, `--config-file <PATH>` | **Optional**, refers to a configuration file which contains all parameters in JSON format (see [section below](scenario_convert.md#configuration-files-for-scenario-convert)) |

## Use case overview

In the following, we explain the usage of Scenario-convert in the context of typical use-cases when it comes to scenario creation:

|                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| :arrow_forward: [Create MOSAIC scenario from OSM file](scenario_convert.md#-create-mosaic-scenario-from-osm-file)                                   |
| :arrow_forward: [Create MOSAIC scenario from SUMO file](scenario_convert.md#-create-mosaic-scenario-from-sumo-file)                                 |
| :arrow_forward: [Create/Import routes for existing MOSAIC scenario](scenario_convert.md#-createimport-routes-for-existing-mosaic-scenario)          |
| :arrow_forward: [Import height information to existing MOSAIC scenario](scenario_convert.md#-import-height-information-to-existing-mosaic-scenario) |
| :arrow_forward: [Export database or routes to GeoJson](scenario_convert.md#-export-database-or-routes-to-geojson-files)                             |

---

### **:arrow_forward: Create MOSAIC scenario from OSM file**

There are two ways to create a MOSAIC scenario from an OSM file using scenario-convert.

**1. Using a single call with `--osm2mosaic`:**

The easiest method to create a MOSAIC scenario from OSM data is to use scenario-convert with the argument `--osm2mosaic`. One only needs to
specify the path to the input OSM file. The OSM file will first be used to create a scenario database containing road network information.
Afterward, the MOSAIC scenario folder structure and SUMO connection, edge, network, node, route, and config files are created.
You can also use additional arguments alongside this, for example `--generate-routes`. Because of this argument, routes are generated
randomly and added to the scenario database before the scenario is exported to SUMO files

The call for the described example would look like this: <br>

```shell
scenario-convert.bat --osm2mosaic -i '/path/to/map.osm' --generate-routes 
```

**2. Using two separate calls with the option `--osm2db` and `--db2mosaic` subsequently:**

This works very similarly to the first approach, but gives you more control with handling the database-file. The first call creates the
database from the OSM file. Currently, there is only the database file that contains all relevant road network information. It is possible
to now alter the database, for example by generating routes or including elevation information. How to do both of these is described below.
After all changes to the database are completed, you can now call scenario-convert a second time using the second argument and create a
complete MOSAIC scenario structure and SUMO connection, edge, network, node, route, and config files from the database.

Example calls for this approach would look like this: <br>
**First call:**

````shell
scenario-convert.bat --osm2db -i '/path/to/map.osm' -d '/path/to/database.db'
````

The `-d` argument here is optional. If not specified, the database will be created in the same folder that the OSM file is in. <br>

**Second call:**

````shell
scenario-convert.bat --db2mosaic -d '/path/to/database.db' 
````

**Additional arguments usable with `--osm2xxx` arguments**

| Argument                     | Explanation                                                                                                                                                                                                                                        |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--generate-routes`          | Without additional configuration this generates routes between all border points/dead ends in the OSM map. All generated routes will be added to the database                                                                                      |
| `--import-buildings`         | Activates import of building information into the database                                                                                                                                                                                         |
| `--osm-speeds-file <Path>`   | Define a property file which contains speed information which are used to set the speed for OSM ways without a max speed tag                                                                                                                       |
| `--osm-speeds-overwrite`     | If set to true , the maxspeed tags of ways are ignored and replaced by either default values , or by speed information defined via the `--osm-speeds-file` argument                                                                                |
| `--skip-graph-cleanup`       | Turns off the removal of unconnected parts from the main traffic network graph . Since several components of MOSAIC require one main graph without disconnected ways and nodes, this option should be used only if the cleanup procedure is faulty |
| `--skip-osm-filter`          | Skips automatic filtering of the OSM file                                                                                                                                                                                                          |
| `--skip-turn-restrictions`   | Ignore all defined turn restrictions on OSM import                                                                                                                                                                                                 |
| `--split-connections`        | Connections are built for each adjacent node during OSM Import instead of having geometry nodes. This results in much larger networks                                                                                                              |
| `-s; --sumo-prefix <String>` | Prefix for the SUMO files generated during the process (uses database name when not defined)                                                                                                                                                       |

---

### **:arrow_forward: Create MOSAIC scenario from SUMO file**

If you want to convert an already existing SUMO scenario to a usable MOSAIC scenario, you can do that in two distinct ways as well.

**1. Using a single call with `--sumo2mosaic`:**

The concept of this is the same as for creating a MOSAIC scenario from an OSM file. Simply specify the path to the sumocfg file of your
existing SUMO scenario. Scenario-convert will then wrap the existing SUMO scenario in a MOSAIC scenario by moving SUMO files to the MOSAIC
scenarios sumo folder, creating all other relevant MOSAIC files and importing the net and route file into the MOSAIC database.

This examples call would look like this:

```shell
scenario-convert.bat --sumo2mosaic -i '/path/to/sumo.sumocfg' 
```

**2. Using two separate calls with the option `--sumo2db` and `--db2mosaic` subsequently:**

This works similar to the first approach, but, as for the process from OSM, gives you more control with handling the database file. However,
in this case the functionality of the `--sumo2db` argument is defined by further arguments in the call. For creating a new database from a
SUMO network file pass the `-i` argument with a path to a SUMO network file. This way, the network file is read and its contents written
into a new database file. The second call with `--db2mosaic` now creates a complete MOSAIC scenario structure, while keeping the existing
SUMO files from the original scenario.

Example calls for this approach would look like this: <br>
**First call:**

````shell
scenario-convert.bat --sumo2db -i '/path/to/sumo.net.xml' --import-zone 33n
````

The import zone is important for the projection of the coordinates. It is the UTM zone your scenario lies in.

**Second call:**

````shell
scenario-convert.bat --db2mosaic -d '/path/to/database.db'
````

**Additional Arguments with `--sumo2xxx`:**

| Argument                     | Explanation                                                                                  |
|------------------------------|----------------------------------------------------------------------------------------------|
| `--import-lat <Double>`      | Center latitude of imported region. Used to project coordinates                              |
| `--import-lon <Double>`      | Center longitude of imported region. Used to project coordinates                             |
| `--import-zone <String>`     | UTM zone of location for projecting coords in default format (e.g. 32u)                      |
| `-s; --sumo-prefix <String>` | Prefix for the SUMO files generated during the process (uses database name when not defined) |

### **:arrow_forward: Create/Import routes for existing MOSAIC scenario**

Often you have an existing database representing your road network but no routes for vehicles yet. Routes can be either created from
scratch or imported from SUMO route files using scenario-convert.

**1. Import a SUMO route file into a database:**

Importing a route file is possible with just a single call of scenario-convert. This call reads the routes in the file, saves each routes id
and edges, and creates objects in the database for them.

````shell
scenario-convert.bat --sumo2db -d '/path/to/database.db' -i '/path/to/routefile.rou.xml'
````

{{% alert note %}}
Notice that the argument `--sumo2db` has different functionality depending on if the input `-i` is a **route.rou.xml** or a
**network.net.xml** file. Both load the respective routes/network into the database if the `-d` argument is given as well, but it is not
possible to create a database from a route file by omitting the `-d` argument, which is possible for network files.
{{% /alert %}}

**2. Creating new routes:**

Creating new routes with scenario-convert can be done in two ways. Either by creating random routes through the network using only
`--generate-routes` or by additionally specifying start and end point of the route you want to create. Random routes are always created
based on road end points in your network, meaning they always range from one edge of the map to another or from dead end to dead end.
You can also specify start and end point of a route, either in geo coordinates or through node IDs.

The following examples showcase multiple approaches to generating routes using scenario-convert.

**Generate X random end-to-end routes using `-g / --generate-routes`:**

If you want to generate many routes fast this option is the best possibility to do that.

````shell
scenario-convert.bat --generate-routes -d '/path/to/database.db' --max-number-of-routes X
````

You can cap the number of routes generated by passing the argument `--max-number-of-routes X`, where X is a positive integer.
Scenario-convert then generates a maximum of X routes. If you don't pass this argument, all possible routes for your road network are
calculated. This is not recommended for large networks.

**Calculate a single route from point A to point B:**

In contrast to the example above, if a start and end point is given, scenario-convert only calculates a single route between these points
if `--max-number-of-routes` is not given.

````shell
scenario-convert.bat --generate-routes -d '/path/to/database.db' --route-begin-latlon 52.526371,13.314075 --route-end-latlon 52.512879,13.320099
````

The points can be given in geo coordinates or as node IDs. If given in geo coordinates, make sure the coordinates are within the area of 
your scenario network.

**Calculate X alternative routes from point A to point B:**

If start and end point are given and `--max-number-of-routes X` is passed, where X is a positive integer, scenario-convert calculates
multiple routes between the points, with a maximum of X alternatives. It is not guaranteed, that X routes are calculated.

````shell
scenario-convert.bat --generate-routes -d '/path/to/database.db' --route-begin-latlon 52.526371,13.314075 --route-end-latlon 52.512879,13.320099 --max-number-of-routes X
````

**Calculate all possible routes between a list of points:**

If you want to calculate routes between multiple points you can pass a list of node IDs with the argument `--route-matrix`.
This will generate all possible routes between all points in this list.

````shell
scenario-convert.bat --generate-routes -d '/path/to/database.db' --route-matrix '12345,98765,13579' 
````

**3. View all routes in a database:**

If you have already created routes and want to view them without using external tools you can use the argument `--list-routes`.
In combination with `-d` to set the path to the database you want to view, this outputs a summary of all routes in the database consisting
of route ID, start node ID, end node ID, and route length in meter.

**View routes in a database by using the follwing call:**

````shell
scenario-convert.bat --list-routes -d '/path/to/database.db'
````

**Additional arguments with `--generate-routes`:**

| Argument                               | Explanation                                                                                          |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| `--clear-routes`                       | Clear existing routes from the database before adding new ones                                       |
| `--list-routes`                        | List all existing routes from the scenario database                                                  |
| `--route-begin-lat <Double>`           | Latitude of route starting point, needs `--route-begin-lon` as well                                  |
| `--route-begin-lon <Double>`           | Longitude of route starting point, needs `--route-begin-lat` as well                                 |
| `--route-begin-latlon <Double,Double>` | Latitude and longitude of route starting point                                                       |
| `--route-begin-node-id <String>`       | OSM node id of the starting point (use instead of lat/lon)                                           |
| `--route-end-lat <Double>`             | Latitude of route end point, needs `--route-end-lon` as well                                         |
| `--route-end-lon <Double>`             | Longitude of route end point, needs `--route-end-lat` as well                                        |
| `--route-end-latlon <Double,Double>`   | Latitude and longitude of route end point                                                            |
| `--route-end-node-id <String>`         | OSM node id of the end point (use instead of lat/lon)                                                |
| `--route-matrix <String,String>`       | Calculates all possible routes starting and ending at the given nodes, given as comma-separated list |

### **:arrow_forward: Import height information to existing MOSAIC scenario**

Importing height information into the database can be done with the argument `--srtm2db`. It requires a SRTM file, whose elevation data
is then written to the nodes of an existing database.

**An example call would look like this:**

````shell
scenario-convert.bat --srtm2db -i '/path/to/srtm.geotiff' -d '/path/to/database.db'
````

### **:arrow_forward: Export database or routes to GeoJson files**

Sometimes you might want to export the contents of your database into a GeoJSON format. For example for visualizations of your road network
using QGIS.

**Use this scenario convert call for this purpose:**

````shell
scenario-convert.bat --db2geojson -d '/path/to/database.db'
````

This will export the database content into a .geojson file with the same name as the database.

## Configuration-files for scenario-convert

Scenario-convert offers a way to safe your conversion-parameters in a `JSON` configuration file using
the option `-c` or `--config-file`.  
The following listing shows how to save the options used in the example above:

```
{{< include_file path="files/steglitz_config.json" >}}
```

## Speed-files

Below you can find a properties file which can be used during the import of OSM data
in order to define speeds for ways, which do not have a maxspeeds-tag defined. For this purpose use the
option `--osm-speeds-file <FILE>`. In the speed properties file, for each way type a speed value can
be defined, according to the OSM [`highway`](http://wiki.openstreetmap.org/wiki/Key:highway) key.

```
{{< include_file path="files/car-speeds.properties" >}}
```
