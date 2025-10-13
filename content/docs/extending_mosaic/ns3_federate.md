---
title: ns-3 Federate Development Setup
linktitle: ns-3 Federate
toc: true
type: docs
date: "2021-07-23T20:00:00+02:00"
draft: false
weight: 61
menu:
  docs:
     parent: extending_mosaic
---

{{% alert warning %}}
Temporary Notice: This page and the corresponding contents are only valid starting with upcoming release 25.1, or for the developer version from GitHub.
{{% /alert %}}

The following instructions steps lead you to a common **development setup for the Eclipse MOSAIC ns-3 Federate**.

{{% alert note %}}
**For the Eclipse MOSAIC ns-3 Federate development you should be familiar with:**
- Eclipse MOSAIC
- C++ development
- ns-3
{{% /alert %}}

## Prerequisites and Dependencies

Please have a look at the [ns-3 section](/docs/simulators/network_simulator_ns3) for system requirements and dependencies. You should install all dependencies to run `ns3_installer.sh` before continuing here, developing ns-3 or the ns-3-federate.

The current version is tested on Ubuntu 22.04 (we used WSL). 
On Ubuntu 24.04 we ran into problems when compiling ns-3.36.1. 
An upgrade to newer version is in the works.

## Federate Development with the Bundle (and `ns3_installer.sh`)

- Compile or download the bundle `eclipse-mosaic-{{< version of="mosaic_snapshot" >}}.zip`
- Extract the bundle and go to `<bundle>/bin/fed/ns3`
- Run the script `./ns3_installer.sh --keep-src` 
- Run a simulation to test that you did the setup correctly  (e.g., the `Tiergarten` scenario can be activated with `ns-3`).

**For federate development**
- Do your changes to files in `federate/src`
- Recompile and deploy the federate.
```bash
cd federate
make -j1
mv ./bin/Debug/ns3-federate ..
```

**For ns-3 development**
- Change the source files along to your needs
- Recompile ns-3
```bash
cd ns-allinone-{{< version of="ns3" >}}/ns-{{< version of="ns3" >}}
./ns3 build
```

## Federate Development with the IDE

This part mainly does most of the steps from `ns3_installer.sh` but manual and thereby more flexible. This setup even works across operating systems: You can start MOSAIC from the IntelliJ IDE in Windows, whereas the federate and ns3 are installed in some Ubuntu inside WSL.

### ns-3

Download, extract, and compile ns-3.

```bash
wget https://www.nsnam.org/releases/ns-allinone-{{< version of="ns3" >}}.tar.bz2
tar xjf ns-allinone-{{< version of="ns3" >}}.tar.bz2
cd ns-allinone-{{< version of="ns3" >}}/ns-{{< version of="ns3" >}}
./ns3 configure
./ns3 build
```

### ns-3-federate and premake

- Clone the federate repository. Clone it into the same folder where your `ns-allinone` is located. This relative location important for the linking paths.
- Download the premake5 binary (in this case for Linux) and put it into the federate folder
- Run premake and thereby generate the makefiles
- Compile the federate (Building in parallel might cause errors, so the use of one thread with parameter -j1 is recommended.)

```bash
git clone https://github.com/mosaic-addons/ns3-federate
wget https://github.com/premake/premake-core/releases/download/v5.0.0-beta1/premake-5.0.0-beta1-linux.tar.gz
tar xvf premake-5.0.0-beta1-linux.tar.gz
cp premake5 ./ns3-federate
cd ns3-federate
chmod +x premake5 # sometimes necessary
./premake5 gmake --generate-protobuf
make -j1
```


**Note:**
- When your ns-3-federate and `ns3` folder are not in the same location you have to adapt the paths in `premake.lua` before generating the makefiles (use absolute paths and not `~` for your home dir), and adapt the paths in `run_manually.sh`

### Run the simulation

- Prepare the scenario: In the `scenario_config.json` enable ns3 (and probably disable SNS)
- Copy configuration files: Put the `<scenario>/ns3/ns3_federate_config.xml` into the `ns3-federate` folder
- In MOSAIC's `etc/runtime.json` adjust the `ns3` section:

```
...
"deploy": false,
"start": false,
...
```
- Run the federate with the script `bash run_manually.sh`, this will now listen for the startup of your MOSAIC simulation
- Run your MOSAIC simulation from the IDE

**Note:**
- When starting both MOSAIC and ns-3-federate separately, the MOSAIC log (CommunicationDetails.log) will remain empty. Instead you have the log output on your console.

### Developing

**For federate development**
```bash
cd ns3-federate
# make your changes in src/
make -j1
bash run_manually.sh
# start MOSAIC from the IDE
```

**For federate debugging**
```bash
sudo apt install gdb

cd ns3-federate
make -j1
gdb ./bin/Debug/ns3-federate
set env LD_LIBRARY_PATH /path/to/ns-3.36.1/build/lib
break mosaic-node-manager.cc:319
run --port=5011 --cmdPort=0 --configFile=ns3_federate_config.xml
# start MOSAIC from the IDE
# do your GDB debugging
```

**For ns-3 development**
```bash
cd ns-allinone-{{< version of="ns3" >}}/ns-{{< version of="ns3" >}}
# make your changes
./ns3 build
cd <ns3-federate>
bash run_manually.sh
# start MOSAIC from the IDE
```
