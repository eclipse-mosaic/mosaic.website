---
title: Network Simulator ns-3
linktitle: Network - ns-3
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 50
menu:
  docs:
    parent: simulators
---

As the successor of the well-known _Network Simulator 2_, **ns-3** is a discrete-event communication simulator for Internet systems,
wireless networks and more. In connection with Eclipse MOSAIC, especially the IEEE 802.11(p) capabilities are employed for Ad-hoc communication.

|                          |                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------|
| **Operating system**     | GNU/Linux (recommended), _FreeBSD_, _macOS_, _Microsoft Windows (with WSL or Docker)_ |
| **Written in**           | C++ (core) and Python (bindings)                                                      |
| **License**              | Free software: GNU GPLv2                                                              |
| **Website**              | {{< target-blank "http://www.nsnam.org/" "http://www.nsnam.org/" >}}                  |
| **Supported version(s)** | {{< version of="ns3" >}}                                                              |
| **Dependencies**         | libprotobuf {{< version of="ns3_protobuf" >}}                                         |
|                          | libxml2                                                                               |
|                          | libsqlite3                                                                            |

## Installation

ns-3 is primarily developed on and for GNU/Linux platforms.

On other platforms like Windows you should consider
- the [Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl/)
- the installation in [Docker environment](#installation-in-docker-environment)
- the installation of Linux in a virtual machine environment, such as
{{< target-blank "VMware" "https://www.vmware.com/products/workstation-player.html" >}} or {{< target-blank "VirtualBox" "https://www.virtualbox.org/" >}}.

For more information on how to set up ns-3 with Eclipse MOSAIC, please just refer to the following section. We
prepared an installation script, which manages most of the required work.


### Prerequisites

For GNU/Linux platforms, the minimal requirements to run basic simulations are a **gcc** or **clang** compiler
and a **Python** interpreter. At least you need the following packages to be installed:

***Minimum requirement:***
```plaintext
gcc
g++
python3
python3-dev
```

For a complete list of required packages for different distributions, please refer to the ns-3 installation
guide: {{< target-blank "https://www.nsnam.org/wiki/Installation" "https://www.nsnam.org/wiki/Installation" >}}

Please make also sure the following libraries are installed before running the installation script:
* libxml2
* libsqlite3
* libprotobuf {{< version of="ns3_protobuf" >}}

### Run the installation script

{{% alert note %}}
ns-3 requires several packages to be installed on your computer. You will
need to ensure, that all required libraries are present on your system before proceeding. You
may need superuser permissions to install packages.
{{% /alert %}}

{{% alert note %}}
If your local ClientServerChannel protocol version (protobuf files) does not fit the required one, the installation
may fail with an error. In that case, you can run the install script with the `-p` flag. This will
rebuild the protobuf files during installation and allow it to proceed correctly.
{{% /alert %}}

To ease the installation of ns-3 for Eclipse MOSAIC, the installation process has been delegated to an installation
script, called `ns3_installer.sh` that can be found in the associated ns-3 federate folder.

```plaintext
└─ mosaic/bin/fed/
   └─ ns3
      ├─ Dockerfile.sh ................... Dockerfile for ns-3 federate
      └─ ns3_installer.sh ................ Installation script for ns-3
```

**The ns-3 installation script accomplishes the following tasks:**
1. Download ns-3 tarball from the official sources
2. Download the ns-3 federate for Eclipse MOSAIC.
3. Apply a patch to ns-3 in order to make it work with Eclipse MOSAIC.
4. Add the ns-3 federate to the waf build system.
5. Configure and build the patched ns-3 with the ns-3 federate.

**In order to start the simulation, the following steps need to be performed:**
1. Set up the `ns3_federate_config.xml`-file in the scenario folder (see section [Configuration](/docs/simulators/network_simulator_ns3#configuration)). An example `ns3_federate_config.xml` - file is shipped with the Tiergarten scenario.
2. At last ns-3 has to be activated in the `mosaic_config.xml` and the simulation can be started.


{{% alert note %}}
Using the installer script will reinstall (rebuild) ns-3 and the federate because it is designed for a user environment and not for federate development.
Read [here](/docs/extending_mosaic/ns3_setup) for futher information on how to set up a development environment for the ns-3 federate
{{% /alert %}}

### Installation in Docker environment

This guide gives instructions to execute the ns-3 federate inside a docker container. If you already installed
ns-3 on your machine following the steps before, you can skip this section.

Docker is a new approach to execute software. More precisely, it "wraps software in a complete filesystem
that contains everything it needs to run: code, runtime, system tools, and system libraries". As a result, the
software is executed within a container and its execution does not rely on the environment the container is running in.

In context of Eclipse MOSAIC, this approach allows to execute ns-3 within a docker container. The user does not
need to manually install ns-3 and can even run ns-3 on Windows hosts.
1. Install Docker ≥ {{< version of="ns3_docker" >}} on your machine.
2. To get everything to work, please make sure to execute the following steps depending on your operating system:
    * Windows - In the settings, share the drive where Eclipse MOSAIC is installed on. You may need to restart docker in the reset tab.
    * Linux - Make sure your user account belongs to the unix-group `docker`. You may need to restart your machine.
3. Switch to the location of the Dockerfile in `<mosaic>/bin/fed/ns3`
4. Execute the following command on command line:  
    `docker build -t ns3-federate` .
    This could take a while to finish.
5. Enter the name of the docker image `etc/runtime.json` in the `ns3`-section into the property `dockerImage`:  
```json
"federates": [
   ...
   {
      "id": "ns3",
      "dockerImage": "ns3-federate",
      ...
   },
   ...
]
```

You can test the installation of your docker image with the Tiergarten scenario, by activating ns3 in the `scenario_config.json`.

## Configuration

The whole ns-3 specific configuration is done via the files `ns3_config.json` and `ns3_federate_config.xml`
in the scenario folder.

```plaintext
└─ <scenario_name>
   └─ ns3
      ├─ ns3_config.json ................. Ambassador configuration file.
      └─ ns3_federate_config.xml ......... Federate configuration file.
```

The `ns3_config.json` mainly allows the configuration of a filter for different message routing options employed in the ambassador.
The current integration of ns-3 does not support all combinations (e.g. no TCP is possible for ad-hoc communication).
Accordingly, changes in this config would require adaptions in the ns-3 integration as well
and are therefore only recommended developers not for users.

The `ns3_federate_config.xml` allows the configuration of all modules in the IEEE 802.11(p) communication stack of ns-3.
The ns-3 propagation
module defines two generic interfaces, namely **PropagationLossModel** and **PropagationDelayModel**,
for the modelling of propagation loss respectively propagation delay.

In the default `ns3_federate_config.xml`, the Wi-Fi device uses the ns-3 standard propagation delay model
`ns3::ConstantSpeedPropagationDelayModel` and the ns-3 standard propagation loss model
`ns3::FriisPropagationLossModel`. Radio propagation models in ns-3 can easily be exchanged with
the ns-3 class registration system (see the ns-3 documentation for details). The Wi-Fi configuration
includes additional parameters, like sending power and antenna gain.

