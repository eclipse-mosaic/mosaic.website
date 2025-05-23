---
title: Create a new Scenario
linktitle: create_new_scenario
toc: false
type: tutorials
date: "2023-08-11T00:00:00+01:00"
draft: false
pagination_prev: barnim_develop_applications
pagination_next: tiergarten_communication
---

In this tutorial, we will provide all the information needed to create a new Eclipse MOSAIC scenario from scratch with the help of the 
**scenario-convert** tool.

{{% alert learning_objectives %}}
With this tutorial you learn...  
* How to create new scenarios.
* How to use scenario-convert and with OSM-files.
* How to clean map-data before generating the simulation map.
* How to import routes into your scenario.
{{% /alert %}}

1) Use the tool scenario-convert to create a custom simulation scenario. 
   This scenario includes the road network, vehicles and their routes through the network, locations of road side units, communication
   properties, and the like.
2) Implement application models to be integrated with the [Application Simulator](/docs/simulators/application_simulator). 
[Applications are developed](/docs/develop_applications) in JAVA and implement pre-defined interface classes allowing them to have access to entity specific functions, e.g. 
   for exchanging V2X messages, influencing the vehicle's behavior, or controlling road infrastructure.
3) Deploy your developed applications onto simulation entities using the [Mapping Configuration](/docs/simulators/application_mapping). Each entity, e.g. vehicle, server, or 
   RSU, is mapped with an application model developed in the previous step.
4) Run your simulation and evaluate them with the help of pre-bundled [Output Generators](/docs/visualization/filevis).

## scenario-convert

For creating new scenarios, but also for importing and exporting data from
external sources like OpenStreetMap, SUMO etc. into your existing scenarios, we provide the tool **scenario-convert**.  

{{% alert extended %}}
**scenario-convert** is part of **[MOSAIC Extended](/download#overview)**. 

However, you can use scenario-convert **for free** to generate scenarios which are executable with Eclipse MOSAIC. **[Get it here](https://www.dcaiti.tu-berlin.de/research/simulation/download/).**
{{% /alert %}}

{{< button icon="download" type="primary" link="https://www.dcaiti.tu-berlin.de/research/simulation/download/" 
title="Download scenario-convert from DCAITI mirror" >}}

1. **Download the `scenario-convert-{{< version of="mosaic" >}}.zip/tar.gz` bundle from the mirror above.**
2. Extract the package to an arbitrary path. It is recommended to extract the package in the very same directory as MOSAIC (`<mosaic-root>`).
3. Use the call `scenario-convert.bat/sh` on the command line to execute the tool.

`scenario-convert` will create a database, which is the basis for all map-related tasks performed by Eclipse MOSAIC (e.g. navigation,
route calculation...).  
Based on a MOSAIC database, scenario-convert can export the data to SUMO-conform formats.
Furthermore, one can choose, whether to use routes generated by `scenario-convert`, use existing
routes or build own routes via route generation tools (e.g., DUAROUTER by SUMO).

This chapter intends to highlight the most common workflows for the work with `scenario-convert`.
We will be using [steglitz.osm](files/steglitz.osm) OSM-file for most of the
use cases. Best is to save `steglitz.osm` in the same directory where `scenario-convert` is located. 
So feel free follow along with the steps to get a better understanding on how the `scenario-convert`-script functions.

{{% alert note %}}
 [Here](/docs/scenarios/scenario_convert) you can find the complete scenario-convert reference. 
{{% /alert %}}

{{< figure src="images/osm_uncleaned.png" title="OSM-File of Steglitz" numbered="true" >}}

## Creating a complete Eclipse MOSAIC-scenario from an OSM-file with one command

This is the most straight forward way to create a scenario from your OSM-file.
We will use the option `--osm2mosaic`, which is a combination of the options `--osm2db`
and `--db2mosaic`.  
Let's start off by showing you what a complete call could look like:

```bash
scenario-convert.sh --osm2mosaic -i steglitz.osm
```

{{% alert note %}}
In this section we use the scenario name `steglitz.*` as a placeholder for `path/to/steglitz.*`.
{{% /alert %}}

Please be aware that the scenario generated with the call above contains no routes yet, see [Import Routes](#importing-routes-to-your-scenario) for more details.
The complete call achieves a couple of things. First off the script is going to create a SQLite-database,
which is used by Eclipse MOSAIC. Furthermore, a directory will be created, which should look like this:

```plaintext
└─ <working-directory>
   └─ steglitz
       ├─ steglitz.osm
       ├─ application
       |  └─ steglitz.db
       ├─ cell
       |  ├─ cell_config.json
       |  ├─ network.json
       |  └─ regions.json
       ├─ environment
       |  └─ environment_config.json
       ├─ mapping
       |  └─ mapping_config.json
       ├─ ns3
       |  ├─ ns3_config.json
       |  └─ ns3_federate_config.xml
       ├─ omnetpp
       |  ├─ omnetpp.ini      
       |  └─ omnetpp_config.json
       ├─ output
       |  └─ output_config.xml
       ├─ phabmacs
       |  └─ phabmacs_config.json
       ├─ sns
       |  └─ sns_config.json
       ├─ sumo
       |  ├─ steglitz.net.xml
       |  └─ steglitz.sumocfg
       └─ scenario_config.json .................. General configuration of the simulation scenario
```

Let's walk through all these files:
1. First the `steglitz.db` will be created using the `steglitz.osm`-file.
2. The `steglitz.db` will be used to create `steglitz.con.xml`, `steglitz.edg.xml` and `steglitz.nod.xml`, which are files used by SUMO.
3. [SUMO Netconvert](https://sumo.dlr.de/wiki/NETCONVERT) is used to create `steglitz.net.xml`, which is a [network representation](https://sumo.dlr.de/wiki/Networks/SUMO_Road_Networks) in SUMO.
4. Now the SUMO-configuration `steglitz.sumocfg` is written.
5. Afterward, the `mapping_config.json` and `scenario_config.json` are created and all files are moved to the right place.
In the `scenario_config.json` values like the center coordinate will automatically be set using data from the SUMO related files.

While this is technically sufficient to start working on your scenario, like [importing routes](#importing-routes-to-your-scenario) to it, there are a couple of other things
you can do to achieve better results.

### Clean the OSM-file using Osmosis
By default, Osmosis will be used to create a new OSM-file with the suffix `_cleaned`. The created
file will contain much less clutter and usually is better suited for simulation purposes.
Check the images below to see the difference the clean-up process can make.

<div class="row">
    <div class="col-6">
        {{< figure src="images/osm_uncleaned.png" title="Uncleaned OSM-file" numbered="true" >}}
    </div>
    <div class="col-6">
        {{< figure src="images/osm_cleaned.png" title="Cleaned OSM-file" numbered="true" >}}
    </div>
</div>
<div class="row">
    <div class="col-6">
        {{< figure src="images/netfile_uncleaned.png" title="Uncleaned Net-file" numbered="true" >}}
    </div>
    <div class="col-6">
        {{< figure src="images/netfile_cleaned.png" title="Cleaned Net-file" numbered="true" >}}
    </div>
</div>

To avoid "cleaning" the OSM-file, please use the option "skip-osm-filter".
```bash
scenario-convert.sh --osm2mosaic -i steglitz.osm --skip-osm-filter
```

### Generating Routes
The scenario-convert also offers the option `--generate-routes`, which will generate
a route-file, given some additional information. For example purposes we will run the
 command below. In case you generated the `steglitz scenario` in one of the steps above already, please delete or rename the `steglitz` directory and run:
```bash
scenario-convert.sh --osm2mosaic -i steglitz.osm --generate-routes
--route-begin-latlon 52.4551693,13.3193474 --route-end-latlon 52.4643101,13.3206834 --max-number-of-routes 3
```
This will calculate up to three routes between the two given coordinates. 

Alternatively you can use the following command in order to generate routes with node-id's as start and end point, which can be found in the `steglitz.nod.xml` file.

```bash
scenario-convert.sh --osm2mosaic -i steglitz.osm --generate-routes
--route-begin-node-id 267350668 --route-end-node-id 313139970 --max-number-of-routes 3
```
See [scenario-convert documentation](/docs/scenarios/scenario_convert) for all command line options.

### Conclusion
This wraps up one of the main workflows with the scenario-convert-script.
A quick reminder of what we achieved:
- Cleaned up an OSM-file to only contain relevant data.
- Converted that OSM-file to formats that Eclipse MOSAIC/SUMO can handle.
- Created the project structure for a scenario.
- Calculated routes between two coordinates.

With all of this you can now start further developing your scenario. For a more detailed description on the next steps
please have a look at [Additional Scenario Configuration](/docs/scenarios/scenario_configuration) and
[Application Development](/docs/develop_applications).

You can now move your `steglitz` directory to `/scenarios` and test it by running
`./mosaic.sh -s steglitz` on Linux/macOS or `.\mosaic.bat -s steglitz` on Windows.

{{% alert note %}}
If you've reached this part of the tutorial, you finished the main part of this tutorial.
Below we'll introduce how `scenario-convert` can be utilized to import routes instead of generating them.
{{% /alert %}}

## Importing Routes to your scenario

Often times, your routes won't be created by the scenario-convert script.
Instead, they will have to be imported from another source.
As an example for this use case, we generated some routes for the steglitz-scenario using SUMO's
[duarouter](http://sumo.dlr.de/wiki/DUAROUTER), which you can find [here](files/steglitz.rou.xml). We'll start with
only the `steglitz.osm` and `steglitz.rou.xml` files:

```plaintext
└─ <working-directory>
   ├─ steglitz.osm
   └─ steglitz.rou.xml
```

### Creating the database
We'll start off by solely creating the database and applying OSMOSIS with the following command:
```bash
scenario-convert.sh --osm2db -i steglitz.osm
```
The directory should look like this:

```plaintext
└─ <working-directory>
   ├─ steglitz.db
   ├─ steglitz.osm
   ├─ steglitz.rou.xml
   └─ steglitz_cleaned.osm
```

### Importing the route-file

Let's import our routes into the database.  
We achieve this by calling:
```bash
scenario-convert.sh --sumo2db -i .\steglitz.rou.xml -d .\steglitz.db
```
Now all new routes are imported into our database. The following image shows a visualization of one of
the created routes.

{{< figure src="images/steglitz_route.png" title="Visualization of one of the routes" numbered="true" >}}

### Creating the scenario

The final step is to create the scenario from the files we created so far.

```bash
scenario-convert.sh --db2mosaic -d .\steglitz.db
```
Instead of copying our SUMO-files this will generate all necessary files and move them into an Eclipse MOSAIC-conform
folder structure:

```plaintext
└─ <working-directory>
   ├─ steglitz.osm
   └─ steglitz
      ├─ application
      |  └─ steglitz.db
      ├─ mapping
      |  └─ mapping_config.json
      ├─ sumo
      |  ├─ steglitz.net.xml
      |  └─ steglitz.sumocfg
      └─ scenario_config.json
```

As you can see the resulting folder structure looks just like the final output from the first workflow described.

### Conclusion

You should now know how you can manually add routes to your scenario and have a deeper understanding of the way that
some of the script parameters work.

### Attached Files
A list of all attached files in this chapter:
- [Steglitz OSM-file](files/steglitz.osm)
- [Steglitz Route-file](files/steglitz.rou.xml)
