---
title: Download Eclipse MOSAIC
linktitle: Download
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 10
menu:
  docs:
    parent: getting_started
---

{{< button icon="download" type="primary" link="https://www.dcaiti.tu-berlin.de/research/simulation/download/" title="Download Eclipse MOSAIC from DCAITI mirror" >}}

1. **Download the `eclipse-mosaic-{{< version of="mosaic" >}}.zip/tar.gz` bundle from the mirror above.**
2. Extract the package to an arbitrary path. This installation path is referenced as `<mosaic-root>` throughout the entire document.
3. **Install additional software** required by the simulation (see below), e.g. [Eclipse SUMO](https://www.eclipse.org/sumo)

### Folder Content

```plaintext
└─ <mosaic-root>
   ├─ etc
   |   ├─ hosts.json .................. Configuration of the execution host, e.g. temporary directory.
   |   ├─ logback.xml ................. Configuration of log files and levels.
   |   └─ runtime.json ................ Configuration of all Ambassadors and Federates coupled with the MOSAIC
   ├─ lib ............................. Directory with all Java compiled libraries required for MOSAIC.
   ├─ logs ............................ Directory with log files.
   ├─ scenarios ....................... Directory containing all simulation scenarios.
   ├─ tools ........................... Additional tools, like the HTML Visualizer.
   ├─ CONTRIBUTING.md 
   ├─ LICENSE 
   ├─ mosaic.bat ...................... Start script for Windows systems.
   └─ mosaic.sh ....................... Start script for GNU/Linux/Mac systems.
```

### Additional Software

Each simulation with Eclipse MOSAIC requires additional software. As MOSAIC is written in JAVA, first, a JAVA runtime environment (JRE)
needs to be installed for your operating system.
Furthermore, each simulation requires additional simulators to be installed, for example the traffic simulation tool Eclipse SUMO or the
communication simulators OMNeT++ / INET or ns-3.

The following table gives an overview of supported environments and simulators. Please make sure that you install those versions only.

<style>
table {
  display: table;
}
</style>

|                                             Component                                              | Required |                                                                    |                         Version                         |                                                                      |
|:--------------------------------------------------------------------------------------------------:|:--------:|-------------------------------------------------------------------:|:-------------------------------------------------------:|:---------------------------------------------------------------------|
| **[Java](https://adoptium.net/de/temurin/releases?version={{< version of="java_recommended" >}})** |   yes    |          {{< version of="java_no_support" >}}<br/> _not supported_ |     **{{< version of="java" >}}** <br/> _supported_     | {{< version of="java_limited_support" >}} <br/> _not tested_         |
|                                                                                                    |          |                                                                    |                                                         |                                                                      |
|                          **[Eclipse SUMO](https://www.eclipse.dev/sumo)**                          |   yes*   |          {{< version of="sumo_no_support" >}}<br/> _not supported_ | **{{< version of="sumo_support" >}}** <br/> _supported_ | {{< version of="sumo_not_tested" >}} <br/> _not tested_              |
|                          **[OMNeT++](https://omnetpp.org/download/old)**                           | optional | {{< version of="omnetpp_no_support_below" >}}<br/> _not supported_ |   **{{< version of="omnetpp" >}}** <br/> _supported_    | {{< version of="omnetpp_no_support_above" >}} <br/>  _not supported_ |
|                         **[INET](https://inet.omnetpp.org/Download.html)**                         | optional |    {{< version of="inet_no_support_below" >}}<br/> _not supported_ |     **{{< version of="inet" >}}** <br/> _supported_     | {{< version of="inet_no_support_above" >}} <br/> _not supported_     |
|        **[ns-3](https://www.nsnam.org/releases/ns-{{< version of="ns3_release_link" >}})**         | optional |     {{< version of="ns3_no_support_below" >}}<br/> _not supported_ |     **{{< version of="ns3" >}}** <br/> _supported_      | {{< version of="ns3_not_tested" >}} <br/> _not tested_               |

{{% alert note %}}
**All provided scenarios require SUMO to be installed.** However, if a different traffic or vehicle simulator
is coupled, SUMO is not necessarily required. Follow [this guide](/docs/simulators/traffic_simulator_sumo) for SUMO installation instructions.
{{% /alert %}}

{{% alert note %}}
An Eclipse MOSAIC bundle is built with every commit on our main branch in
our [<i class="fab fa-github"></i> GitHub repository</a>](https://github.com/eclipse-mosaic/mosaic). You
can access and download this distribution following [this link](https://ci.eclipse.org/mosaic/job/mosaic/job/main/).
{{% /alert %}}

### Update Eclipse MOSAIC

To update Eclipse MOSAIC to a new version, please perform the following steps manually:

* Backup your personal simulation scenarios from MOSAIC's scenarios directory.
* Remove your old MOSAIC installation completely.
* Install MOSAIC by extracting the current binary archive from above.
* Copy your simulation scenarios back to MOSAIC's scenarios directory.
* Take care of possible updates of the used software and simulators from third party (see the Compatibility Matrix above).