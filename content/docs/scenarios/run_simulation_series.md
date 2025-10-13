---
title: Run Simulation Series
linktitle: Run Simulation Series
toc: true
type: docs
date: "2021-08-27T00:00:00+01:00"
draft: false
weight: 40
menu:
  docs:
    parent: scenarios
---

{{% alert extended %}}
The **Simulation Runner** is part of **[MOSAIC Extended](/download#overview)**.  
For further information on licenses, feel free to contact us via **[mosaic@fokus.fraunhofer.de](mailto:mosaic@fokus.fraunhofer.de)**.
{{% /alert %}}

This chapter explains how to efficiently run a simulation series of a scenario using the Simulation Runner.

The Simulation Runner is a tool for automatic simulation parametrization and execution. It provides a comfortable way to configure the execution of multiple simulations, e.g. of simulation series including several runs where only few parameters are changed in each run. With the Simulation Runner, a simulation series can be defined, for example, where the V2X penetration rate is changed in each simulation run. As a result, all  Eclipse MOSAIC simulation runs are started automatically according to the defined parameters.

## Usage

The Simulation Runner is started as follows:
```bash
./simulation-runner.sh -c /scenarios/<scenario_name>/simrunner_config.xml -u <user-id> -p <number_of_parallel_simulations>
```
_On Windows use `simulation-runner.bat`_

The configuration file `/scenarios/<scenario_name>/mosaic/simrunner_config.xml` contains the parameterization information.

With the option `-e` the Eclipse MOSAIC executable can be passed. Passing the Eclipse MOSAIC executable is optional if it is located in the mosaic folder or given in the `simrunner_config.xml`.



## Configuration

The following example shows a complete configuration. Using this configuration, the Simulation Runner would try to run a scenario called Barnim while adapting the mapping, the configuration file of SNS, and Eclipse MOSAIC configuration files. The actual simulation is triggered by generating an adapted scenario folder and calling the same executable the user would normally trigger himself.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

  <!-- basic configuration -->
  <mosaic 
      location="/path/to/mosaic_folder" 
      executable="mosaic.sh" 
      parallelSimulations="2" />
  <scenario 
      name="Barnim" 
      config="scenarios/Barnim/scenario_config.json" 
      persistent="false" 
      progressLogger="false">
      <!--argument>-o TRACE</argument-->
      <!--argument>-w 0</argument-->
  </scenario>
  
  <!-- define connected values for controlled changes -->
  <dimension name="PenetrationRates">
     <parameter name="V2XVehiclePercentage" 
         file="mapping/mapping_config.json" 
         fileFormat="json" item="vehicles[0].types[0].weight" 
         type="ValueList">
         <value>0.0</value>
         <value>0.5</value>
         <value>0.75</value>
     </parameter>
     <parameter name="ClassicVehiclePercentage" 
         file="mapping/mapping_config.json" 
         fileFormat="json" 
         item="vehicles[0].types[1].weight" type="ValueList">
         <value>1</value>
         <value>0.5</value>
         <value>0.25</value>
     </parameter>
     <parameter name="Simulationtime" 
         file="scenario_config.json" 
         fileFormat="json" 
         item="simulation.duration"
         type="ValueList">
         <value>100</value>
         <value>100</value>
         <value>100</value>
     </parameter>
  </dimension>
  
  <!-- define values for automatically permuted simulations -->
  <parameter name="SinglehopRadius" 
      file="sns/sns_config.json" 
      fileFormat="json" 
      item="singlehopRadius" 
      type="ValueList">
      <value>500</value>
      <value>600</value>
  </parameter>
</configuration>
```

The configuration contains three distinct parts of configuration. The system specific definition, the scenario definition and the parametrization. While the first two parts will be explained as part of this section, the parametrization will be explained in it’s own section. 

### System Specific Definition

```xml
<mosaic 
   location="/path/to/mosaic_folder"
   executable="mosaic.sh" 
   parallelSimulations="2" />
```

The system specific part of the configuration above is the only part of the configuration that can be overwritten using a second configuration file. It contains the following attributes:
| Attribute | Description |
|:----------|:------------|
| `location` | Location of the Eclipse MOSAIC installation to use (can be relative or absolute). This attribute can be omitted in case the mosaic executable is in the same location as the simulation-runner executable. |
| `executable` | The provided executable is used to start the simulation. This value is optional and will automatically be set to the default *.bat or *.sh file when omitted. (relative path to `location`) |
| `parallelSimulations` | Defines how many simulations are started in parallel to speed up things. Be aware that you should only use this if you have multiple cores available. This might also coincide with the threads option in the Eclipse MOSAIC configuration. The default is `1`. |

### Scenario Definition

```xml
<scenario 
   name="Barnim" 
   config="scenarios/Barnim/scenario_config.json" 
   persistent="false" 
   progressLogger="false">
   <!--argument>-o TRACE</argument-->
   <!--argument>-w 0</argument-->
</scenario>
```

The scenario definition above contains everything needed to identify the scenario to execute along with parameters that need to be passed to the Eclipse MOSAIC executable. It contains the following attributes:
| Attribute | Description |
|:----------|:------------|
| `name` | The name of the simulation to run. This is expected to be the same as the scenario's folder name and is used to automatically generate the path pointing to the scenario's `scenario_config.json` in a default case. It can be omitted if the config option is set. |
| `config` | This is an optional value containing the concrete path to the scenario's `scenario_config.json`. This can be used if the scenario is not placed in the default scenarios folder (which is discouraged) and overwrites the path generated by the name attribute. Either name or config have to be defined! (path relative to `location`) |
| `persistent` | Defines if the generated files in the Simulation Runner (scenarios with adapted configurations), etc (hohsts.json, logback.xml, runtime.json) and tmp directories will be kept after simulation (value `true`) or deleted (value `false`). |
| `progressLogger` | Writes summary of Simulation Runner execution to std out (value `true`), default is `false`. |
| `stdoutLogger` | Writes log messages from the Eclipse MOSAIC executable to stdout when (value `true`), default is `false`. |

The configuration can also contain a number of additional arguments that are passed to the executable without any changes, separated by spaces.


## Parametrization

The heart of this tool is the parametrization of simulations. Using this, one can define values in the default configuration that are adapted between simulation runs. How many simulation runs are performed is defined by the number of changes configured, enriched with the information about simulation repetitions.
For the example in listing 'Example Configuration' it is expected that the mapping file to be changed has one `vehicles` definition spawning multiple cars with a weighted `type` distribution defining first the equipped and then the unequipped vehicles.

### Parameters

```xml
<parameter name="V2XVehiclePercentage" file="mapping/mapping_config.json" fileFormat="json" item="vehicles[0].types[0].weight" type="ValueList">
  <value>0</value>
  <value>50</value>
  <value>75</value>
</parameter>
```

Each value that should be changed in a run is defined by a `parameter` element identified by a `name` (see listing above). The base value is the `file` which should be changed (relative to the scenario folder). Currently it is needed to define what `fileFormat` is expected from that file, which has impact on the syntax of the item definition which denotes what part of this file should be changed (this will be explained in a bit). The final value is the `type` which denotes how the value change behaves. The child elements depend on this definition and will also be explained in a bit.

`fileFormat` can be one of `xml` or `json`. The item syntax is as followed:
- `xml`: contains an XPath expression (but without the root element)
- `json`: contains an array-style definition of the target value. The value in the listing above would change line 13 in the listing below. (In the first entry of vehicles the attribute weight of the types first entry).

```json
{
  "prototypes": [ { "name": "Car" } ],
  "vehicles": [
    {
      "startingTime": 5.0,
      "targetFlow": 1200,
      "maxNumberVehicles": 250,
      "route": "1",
      "types": [
        {
          "applications": [ "org.eclipse.mosaic.app.tutorial.WeatherWarningApp" ],
          "name": "Car",
          "weight": 0.2
        },
        {
          "name": "Car",
          "weight": 0.8
        }
      ]
    }
  ]
}
```

`type` can currently only have two entries:
- `ValueList`: This expects a list of values as child elements of the parameter. Each value will be used for at least one permutation.
- `IntegerGenerator`: This automatically generates integer values to write as values. The generated numbers can be configured by adding these optional attributes to the parameter element:  
  – `start`: denoting a minimal number where generation should start (this will be the first value), default is `0`.  
  – `step`: denoting the number that will be added to the previous value to generate the new one, default is `1`.
  - `end`: denoting the maximum number (exclusive), default is `start + 10 * step`

### Permutation of parameters

When multiple such parameter elements are defined, a permutation for each specific value definition is generated. Lets say defined are parameters A and B and each parameter has values a and b. The resulting permutations would be:

```
A=a, B=a
A=b, B=a
A=a, B=b
A=b, B=b
```

### Dimensions

Sometimes it is wanted to group some value changes. This can be necessary when changed values need to sum up to a specific value or when specific (named) output files need to be defined. This can be done by enclosing the affected `parameters` into a `dimension` definition. Doing this the values of each `parameter` are connected by their index. For this to work the number of values for each parameter have to be the same. The example in listing 'Example Configuration' utilizes this function to make sure the vehicle percentages sum up to 100. The generated permutations for the dimension enclosed parameters are:

```
V2XVehiclePercentage=0,  ClassicVehiclePercentage=100
V2XVehiclePercentage=50, ClassicVehiclePercentage=50
V2XVehiclePercentage=75, ClassicVehiclePercentage=25
```

When additionally parameters are defined which are not enclosed in the `dimension` tag or another dimension tag is defined, then the permutations will be extended even further. The full permutation for listing 'Example Configuration' is as follows:

```
PenetrationRates(V2XVehiclePercentage=0,  ClassicVehiclePercentage=100), SinglehopRadius=500
PenetrationRates(V2XVehiclePercentage=50, ClassicVehiclePercentage=50),  SinglehopRadius=500
PenetrationRates(V2XVehiclePercentage=75, ClassicVehiclePercentage=25),  SinglehopRadius=500
PenetrationRates(V2XVehiclePercentage=0,  ClassicVehiclePercentage=100), SinglehopRadius=600
PenetrationRates(V2XVehiclePercentage=50, ClassicVehiclePercentage=50),  SinglehopRadius=600
PenetrationRates(V2XVehiclePercentage=75, ClassicVehiclePercentage=25),  SinglehopRadius=600
```



## Additional Information

These are some side effects to remember when working with this tool.

__Ports:__ The Simulation Runner supports automatic assigning of free ports for federates. This means that all federates configured in the `simrunner_config.xml` will get a free port configured by default. This enables multiple simulations to be run simultaneously as long as the federates are started by Eclipse MOSAIC. If some federates are not started through Eclipse MOSAIC but are already running, this will not work.

__Paths:__ Relative paths of the files to be modified will be expanded with the deployment directory of the current simulation run, as defined in the value `location`, to an absolute one.

__Adaptations:__ All values will be modified in copies of the original scenario. The copies will be placed in the Simulation Runner folder in the Eclipse MOSAIC base directory and will be (if not deactivated by configuration) deleted upon completion of the simulation.
