---
title: Run a simulation with Eclipse MOSAIC
linktitle: Run Eclipse MOSAIC
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 20
menu:
  docs:
    parent: getting_started

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)

---

To run a single simulation via Command Line Interface (CLI), call the Eclipse MOSAIC start script with the
following command line arguments. 

### GNU/Linux/macOS

```bash/zsh
./mosaic.sh -s <scenario-name>
./mosaic.sh -c ./scenarios/<scenario_name>/scenario_config.json
```

### Windows

```dos
mosaic.bat -s <scenario-name>
mosaic.bat -c .\scenarios\<scenario_name>\scenario_config.json
```

### Example

The following call starts the example scenario "Barnim" in Eclipse MOSAIC on a Windows machine and opens a
[2D Visualizer in your browser](/docs/visualization):

```dos
mosaic.bat -s Barnim -v
```

{{< figure src="../images/mosaic-barnim.gif" title="Running the pre-bundled example scenario \"Barnim\" with Eclipse MOSAIC" numbered="true" >}}

### CLI Options

The Eclipse MOSAIC start script supports the following arguments:

<style>
table th:first-of-type {
    width: 20%;
}
table th:nth-of-type(2) {
    width: 80%;
}
</style>

| Option | Description                                                                                                                                                                                                                                                                                 |
|:------ |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-c`<br>`--configuration` | The primary configuration file which is scenario dependent and located in the according scenario folder. This file transitively includes other necessary configuration files. Usually you will use the file `<mosaic-root>/scenarios/<scenario_name>/scenario_config.json`.                 |
| `-s`<br>`--scenario` | If the main configuration file of your scenario is located in the default scenario directory of MOSAIC (i.e. in `<mosaic-root>/scenarios/<scenario_name>/scenario_config.json`), this option can be used instead of the `-c` option by passing only the scenario name `-s <scenario_name>`. |
| `-w`<br>`--watchdog-interval` | The interval of the internal alive check (in seconds) which is used by MOSAIC to detect execution stalls. This parameter is not mandatory and it is also possible to turn off the watchdog (`-w 0`) for debug sessions.                                                                     |
| `-o`<br>`--log-level` | Override all specified logging-levels. This option is useful for debugging simulations. For example logging every possible event would be done with `-o TRACE`.                                                                                                                             |
| `-b`<br>`--realtime-brake` | With this parameter, the simulation will be slowed down to a desired Real Time Factor, if possible. When simulations already run slower than real time, this factor will have no effect. For example, use `-b 1` to execute the simulation in real time.                                    |
| `-r`<br>`--random-seed` | The global random seed to set for the simulation run. This is usually defined in the `scenario_config.json`, but can be overridden using this option.                                                                                                                                       |  
| `-v`<br>`--start-visualizer` | Opens a page in your default browser which visualizes all vehicle movements of the simulation on a map. This option only works, if your scenario is configured with the [connector to the 2D Visualizer](/docs/visualization).                                                              |
| `-h`<br>`--help` | Prints a help screen.                                                                                                                                                                                                                                                                       |

While Eclipse MOSAIC is running, it prints some information on the command line:
```shell
[user@gnulinux mosaic]$ ./mosaic.sh -s HelloWorld
2020-09-08 16:46:09,794 INFO  ROOT - Running Eclipse MOSAIC {{< version of="mosaic" >}} on Java JRE v1.8.0_202 (AdoptOpenJdk)
2020-09-08 16:46:09,941 INFO  FederationManagement - Start federation with id 'HelloWorld'
2020-09-08 16:46:09,943 INFO  FederationManagement - Add ambassador/federate with id 'application'
2020-09-08 16:46:09,944 INFO  FederationManagement - Add ambassador/federate with id 'mapping'
2020-09-08 16:46:09,945 INFO  FederationManagement - Add ambassador/federate with id 'sumo'
2020-09-08 16:46:09,946 INFO  FederationManagement - Deploying federate 'sumo' locally in .\tmp\sumo
2020-09-08 16:46:09,962 INFO  FederationManagement - Starting federate 'sumo' locally in .\tmp\sumo
16:46:17 - Simulating: 195000000000ns (195.0s / 1000.0s) - 19.5% (RTF:33.10, ETC:25.2s)
```

The current simulation progress is shown in the following format.
* current wall clock time
* current simulation time in [ns] and [s]
* progress in %
* Real Time Factor (RTF) and Estimated Time to Completion (ETC)

The RTF is the ratio of simulated time to simulation duration in wall clock time, e.g. a real time factor
greater than 1.0 means, the simulation is running faster than real time. Both RTF and ETC are calculated
based on the performance of the last five seconds of the simulation and should only give a rough overview,
how long a simulation can take. Depending on the simulation setup, the values can differ heavily between
start and end of a simulation.


## Gather results

All active simulators as well as the according ambassadors generate certain logging output, depending on their configured logging level.
Therefore, these logs are very helpful to retrace and understand the individual states during the simulation time. The generated log 
files can be found in the `<mosaic-root>/logs/log-<timestamp>` directory.

Moreover, Eclipse MOSAIC offers uniformly formatted and visually prepared results using various 
**[Output Generator](/docs/visualization)** implementations. For  example, the `FileOutputGenerator` generates 
detailed outputs of e.g. vehicle positions, speeds, or message exchanges. 
In the scenarios brought by the latest release, this output mechanism is already configured. 

## Scenario Configuration

The configuration of a simulation scenario consists of a detailed description for each coupled simulator. The main configuration file is found under `scenario_config.json`,
which defines the list of coupled simulators:

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
        "environment": false,
        "cell": false,
        "ns3": false,
        "omnetpp": false,
        "sns": false,
        "sumo": true,
        "output": true
    }
}
```

This way, simulators can be easily added or removed from the simulation. Each of the coupled simulators is configured in the directory
of the `scenario_config.json` file. The release bundle comes with a set of tutorial scenarios, which are described in detail
in the [Tutorials section](/tutorials).
