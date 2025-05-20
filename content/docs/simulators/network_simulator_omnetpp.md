---
title: Network Simulator OMNeT++
linktitle: Network - OMNeT++
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 40
menu:
  docs:
    parent: simulators
---

**OMNeT++** is a simulation platform for discrete-event systems. Even though it is primarily targeted at simulating computer networks and
distributed systems, it cannot be used without any extensions for wireless communication. For this kind of simulations, external model frameworks have to be included.

For the integration to Eclipse MOSAIC the **INET** framework is selected as it provides all models necessary for simulating
Vehicle-2-X communication in ad-hoc mode (based on IEEE 1609 and IEEE 802.11(p)) and even more.
For more information on the **INET** extension you should look closer on the [website](https://inet.omnetpp.org).

|                          |                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------|
| **Operating System**     | GNU/Linux (recommended), _macOS_, _Microsoft Windows (with mingw, WSL, Docker)_     |
| **Written in**           | C++ and NED language                                                                |
| **License**              | free to use for academic use (GPL-like), commercial license available               |
| **Website**              | OMNeT++ &ensp; [https://www.omnetpp.org](https://www.omnetpp.org)<br> INET &emsp;&emsp;&emsp; [https://inet.omnetpp.org](https://inet.omnetpp.org) |
| **Supported version(s)** | OMNeT++ {{< version of="omnetpp" >}} <br>INET {{< version of="inet" >}}             |
| **Dependencies**         | libprotobuf {{< version of="omnetpp_protobuf" >}}<br> protoc <br> bison<br> flex<br> gcc <br> python3 |

## Installation

The official project provides an Windows installation based on mingw, but we don't support this in our setup. 

If you use Windows, please consider
- the installation in [Docker environment](#installation-in-docker-environment)
- the [Windows Subsystem for Linux](https://docs.microsoft.com/windows/wsl/) or the installation of Linux in a virtual machine environment, such as
  {{< target-blank "VMware" "https://www.vmware.com/products/workstation-player.html" >}} or {{< target-blank "VirtualBox" "https://www.virtualbox.org/" >}}.

We prepared an installation script, which manages most of the required work. The script provides two installation types
tailored to the needs of USERs or even DEVELOPERs. With the additional method of using Docker or the purely manual installation,
the following options are possible:

| Type                                                               | Description                     |
|:------------------------------------------------------------------ |:------------------------------- |
| [USER](#installation-for-users)<br>(installation script)           | This installation type addresses those who only want to use the **OMNeT++** network simulator for simulations.<br>Network configurations can also be adjusted.<br><br>If you install the federate with this installation type, **OMNeT++ {{< version of="omnetpp" >}}** and **INET {{< version of="inet" >}}** will automatically be installed inside `<mosaic>/bin/fed/omnetpp` during the installation. |
| [DEVELOPER](#installation-for-developers)<br>(installation script) | The installation for developers addresses those who want to make changes or extend the MOSAIC OMNeT++ Federate.<br><br>This installation type awaits that **OMNeT++ {{< version of="omnetpp" >}}** is already installed on your system and<br>- `PATH` contains `/path/to/omnetpp/bin`<br>- `LD_LIBRARY_PATH` contains `/path/to/omnetpp/lib` |
| [Docker](#installation-in-docker-environment)                      | This installation type addresses those who only want to use the **OMNeT++** network simulator for simulations.<br>Network configurations can also be adjusted.<br><br>If you install the federate with this installation type, **OMNeT++ {{< version of="omnetpp" >}}** and **INET {{< version of="inet" >}}** will automatically be installed bundled as a Docker image. |
| manual installation                                                | This type addresses developers. You can install and build everything manually without the use of the installer script. See [Extending MOSAIC OMNeT++ Federate](/docs/extending_mosaic/omnet_federate) for details on federate build process. |

{{% alert note %}}
If you already have **OMNeT++ {{< version of="omnetpp" >}}** and **INET {{< version of="inet" >}}** installed on your
system, but you simply want to use **OMNeT++** for simulations with Eclipse MOSAIC without developing further the MOSAIC
OMNeT++ Federate, you may also choose the [installation for developers](/docs/simulators/network_simulator_omnetpp#installation-for-developers)
to avoid multiple installations of **OMNeT++** and **INET** on your system.
{{% /alert %}}

Follow the links and download the source code of OMNeT++, INET and the MOSAIC OMNeT++ Federate:

| Software                            | Version                      | Link |
|:----------------------------------- |:-----------------------------|:--------------------------------- |
| **OMNeT++**                         | {{< version of="omnetpp" >}} | [https://github.com/omnetpp/omnetpp/releases/](https://github.com/omnetpp/omnetpp/releases/tag/omnetpp-{{< version of="omnetpp" >}}) |
| **INET**                            | {{< version of="inet" >}}    | [https://github.com/inet-framework/inet/releases](https://github.com/inet-framework/inet/releases/tag/v{{< version of="inet" >}}) |
| **MOSAIC OMNeT++ Federate**         | {{< version of="mosaic" >}}  | [https://github.com/mosaic-addons/omnetpp-federate/releases](https://github.com/mosaic-addons/omnetpp-federate/releases/tag/{{< version of="mosaic" >}}) |

The installer script is located in the OMNeT++ federate folder

```plaintext
└─ mosaic/bin/fed/
   └─ omnetpp
      ├─ Dockerfile.sh ..................... Dockerfile for OMNeT++ federate
      ├─ omnet_installer.sh ................ Installation script for OMNeT++
      └─ rebuild_federate.sh ............... Rebuild script to ease compilation process for Developers
```

Available parameters of `omnet_installer.sh`:

|       | Parameter             | Value                    | Description                                                 |
|:-----:|:--------------------- |:------------------------ |:----------------------------------------------------------- |
| `-t`  | `--installation-type` | `<INSTALLATION_TYPE>`    | Either `USER` or `DEVELOPER`.                               |
| `-o`  | `--omnetpp`           | `<PATH_TO_OMNET_TGZ>`    | Provide the archive containing the OMNeT++ source. If not given, the script tries to download an appropriate version. |
| `-i`  | `--inet`              | `<PATH_TO_INET_TGZ>`     | Provide the archive containing the inet source code. If not given, the script tries to download an appropriate version. |
| `-f`  | `--federate`          | `<PATH_TO_FEDERATE_ZIP>` | Provide the archive containing the OMNeT++-federate and patches for coupling OMNeT++ to Eclipse MOSAIC. If not given, the omnetpp-federate is downloaded by this installation script. |
| `-so` | `--skip-omnetpp`      | -                        | Skip the installation of OMNeT++                            |
| `-si` | `--skip-inet`         | -                        | Skip the installation of INET                               |
| `-q`  | `--quiet`             | -                        | Less output and no interaction required                     |
| `-j`  | `--parallel`          | `<NUMBER_OF_THREADS>`    | Enables make to use the given number of compilation threads.<br>Per default your systems maximum is selected automatically. |
| `-u`  | `--uninstall`         | -                        | Uninstalls the OMNeT++ federate                             |
| `-h`  | `--help`              | -                        | Shows this usage screen                                     |

### Installation for Users

Run the installation script (this takes a few minutes):
```bash
cd <mosaic>/bin/fed/omnetpp
chmod +x omnet_installer.sh`
./omnet_installer.sh \
    --installation-type USER \
    --omnetpp /path/to/omnetpp-{{< version of="omnetpp" >}}-src.tgz \
    --inet /path/to/inet-{{< version of="inet" >}}-src.tgz \
    --federate /path/to/omnetpp-federate-{{< version of="mosaic" >}}.zip
```

The parameters `--omnetpp`, `--inet`, and `--federate` are optional. If not given, the respective source code package will be downloaded by the installation script.  
The installation script should terminate with `SUCESS: The MOSAIC OMNeT++ Federate was successfully installed.` otherwise the installation failed.

### Installation for Developers

Install OMNeT++ and add the correct paths:

```bash
cd ~
tar xvfz omnetpp-5.5.1-src-linux.tgz
cd omnetpp-5.5.1
source setenv
vim ~/.bashrc
    # export PATH=$HOME/omnetpp-5.5.1/bin:$PATH
    # export LD_LIBRARY_PATH=$HOME/omnetpp-5.5.1/lib
./configure WITH_TKENV=no WITH_QTENV=no WITH_OSG=no  WITH_OSGEARTH=no WITH_PARSIM=no
make
```

Run the installation script (this takes a few minutes):
```bash
cd <mosaic>/bin/fed/omnetpp
chmod +x omnet_installer.sh`
./omnet_installer.sh \
    --installation-type DEVELOPER \
    --federate /path/to/omnetpp-federate-{{< version of="mosaic" >}}.zip
```
The installation script should terminate with `SUCCESS: The MOSAIC OMNeT++ Federate was successfully installed.` otherwise the installation failed.

Please continue reading [here](/docs/extending_mosaic/omnet_federate) for more details on setting up a development environment for the OMNeT++ federate.

### Installation in Docker environment

{{% alert note %}}
This is an experimental feature. Please refer to our mailing list if you experience any problems.
{{% /alert %}}

This guide gives instructions to execute the OMNeT++ federate inside a docker container. If you already installed
OMNeT++ on your machine following the steps before, you can skip this section.

Docker is a new approach to execute software. More precisely, it "wraps software in a complete filesystem
that contains everything it needs to run: code, runtime, system tools, and system libraries". As a result, the
software is executed within a container and its execution does not rely on the environment the container is running in.

In context of Eclipse MOSAIC, this approach allows to execute OMNeT++ within a docker container. The user does not
need to manually install OMNeT++ and can even run OMNeT++ on Windows hosts.
1. Install Docker ≥ {{< version of="omnetpp_docker" >}} on your machine.
2. To get everything to work, please make sure to execute the following steps depending on your operating system:
    * Windows - In the settings, share the drive where Eclipse MOSAIC is installed on. You may need to restart docker in the reset tab.
    * Linux - Make sure your user account belongs to the unix-group `docker`. You may need to restart your machine.
3. Switch to the location of the Dockerfile in `<mosaic>/bin/fed/omnetpp`
4. Download `inet-{{< version of="inet" >}}-src.tgz` and `omnetpp-{{< version of="omnetpp" >}}-src-linux.tgz` and place them in `<mosaic>/bin/fed/omnetpp`.
5. Execute the following command on command line:
    `docker build -t omnetpp-federate` .
    This could take a while to finish.
6. Enter the name of the docker image `etc/runtime.json` in the `omnetpp`-section into the property `dockerImage`:
```json
"federates": [
   ...
   {
      "id": "omnetpp",
      "dockerImage": "omnetpp-federate",
      ...
   },
   ...
]
```

{{% alert note %}}
If MOSAIC cannot open a connection to the federate, try to add the option `-Dmosaic.no-detach=true` to the java call in the mosaic.sh file on Linux, or
`-Dmosaic.no-detach=false` in the mosaic.bat file on Windows.
{{% /alert %}}

## Configuration

To use OMNeT++ as network simulator in an Eclipse MOSAIC simulation, open `<scenarioName>/scenario_config.json` and enable OMNeT++:
```json
"federates": {
   ...
   "omnetpp": true,
   ...
}
```
Now, when you run this scenario, Eclipse MOSAIC will automatically start the MOSAIC OMNeT++ Federate.

The main configuration of the MOSAIC OMNeT++ Federate is done within the configuration files `omnetpp.ini` and `omnetpp_config.json` in the scenario:

```plaintext
└─ <scenario_name>
   └─ omnetpp
      ├─ omnetpp.ini ...................... OMNeT++ federate configuration file
      └─ omnetpp_config.json .............. Ambassador configuration file
```

The `omnetpp_config.json` mainly allows the configuration of a filter for different message routing options employed in the ambassador.
The current integration of OMNeT++/INET does not support all combinations (e.g. no TCP is possible for ad-hoc communication).
Accordingly, changes in this config would require adaptions in the OMNeT++/INET integration as well
and are therefore only recommended developers not for users.

The whole OMNeT++ specific configuration is done via the `omnetpp.ini` file. It covers static parts for the
simulator coupling such as the specific Eclipse MOSAIC Event Scheduler and the ScenarioManager. Furthermore,
logging configurations and the typical parameters for the communication layers (MAC, PHY and Radio
Channel) are addressed. The communication parameters are different for vehicles and RSUs. Please refer
to the OMNeT++ documentation on the OMNeT++ homepage for further information about the structure
of the `omnetpp.ini` file.

