---
title: OMNeT++ Federate Development
linktitle: OMNeT++ Federate
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 60
menu:
  docs:
    parent: extending_mosaic
---


This section provides a description how to set up the **OMNeT++ IDE** for the Eclipse MOSAIC OMNeT++ Federate Development.

{{% alert warning %}}
At this point it is awaited, that the [OMNeT++ Federate](/docs/simulators/network_simulator_omnetpp) is successfully installed.
{{% /alert %}}

## Prepare OMNeT++ IDE

1. Create an empty directory somewhere inside your home directory. We will call it `<omnetpp_workspace>` from here on. This directory will be used as a workspace in your OMNeT++ IDE.
2. Open your OMNeT++ IDE by executing `omnetpp` in your terminal.
3. Select `<omnetpp_workspace>` as workspace and continue by clicking `Launch`.
4. Close the "Welcome" screen.
5. Since your workspace is empty, the OMNeT++ IDE will ask you if you want to install the INET framework and OMNeT++ programming examples.  
   {{< figure src="../images/omnetpp-ide-install-inet.png" title="OMNeT++ IDE: prevent automatic installation of INET" numbered="true" >}}
   Uncheck INET and decide for yourself if you want to install the examples.
    * By clicking `OK` the examples are going to be installed into your `<omnetpp_workspace>`
    * If you already have INET installed somewhere you can skip the download and import your existing INET project:
        1. `Cancel` the dialog.
        2. Choose `File` > `Open Projects from File System...`
        3. In the new window choose the directory of your existing INET installation as `Import Source` and click `Finish`  

   The INET framework is supported in version {{< version of="inet" >}}. To ensure the correct version is installed, install it manually:
    1. Download [INET {{< version of="inet" >}}](https://github.com/inet-framework/inet/releases)
    2. Unpack it into the working directory of OMNeT++ IDE (Eclipse)
    3. Rename the unpacked directory `inet4` to `inet`
    4. Insert the directory `inet` as project to OMNeT++ IDE (as described above)
    5. Build INET
6. The project `inet` should now be visible in the `Project Explorer` of your OMNeT++ IDE.
7. Right-click on free space in the `Project Explorer` and choose `New` > `OMNeT++ Project...`  

   {{< figure src="../images/omnetpp-ide-new-project.png" title="OMNeT++ IDE: Create new OMNeT++ Project" numbered="true" >}}
8. In the new window: 
   1. Name the new project `federate`
   2. __Uncheck__ the box before `Use default location`, click `Browse` and select:  
   `<mosaic>/bin/fed/omnetpp/omnetpp_federate_src/src`  
   {{< figure src="../images/omnetpp-ide-new-project-2.png" title="OMNeT++ IDE: Create new OMNeT++ Project" numbered="true" >}}
   3. Click `Next`
9. On the following `Initial Contents` page select `Empty Project` and continue by clicking `Finish`  
   You should now find two projects in the `Project Explorer` of your OMNeT++ IDE: `inet` and `federate`
10. Right-click on the `federate` project and choose `Properties`
    1. Go to `Project references` and __check__ the box before `inet`  
    {{< figure src="../images/omnetpp-ide-project-references.png" title="Choose project references" numbered="true" >}}

That's it! None of the files should now be marked with an error symbol.

---

## Configure Rebuild Configuration

Since the Eclipse MOSAIC OMNeT++ Federate is not a classic OMNeT++ project, it cannot be build regulary with
the OMNeT++ IDE by just clicking on the `Build` button. However, to make the build process easy and intuitive
we provide a simple build script and the following desciption how to configure the OMNeT++ IDE to enable 
building on a single click:

1. In the OMNeT++ IDE select `Run` > `External Tools` > `External Tools Configuration...`
2. Double-click in the left column on `Program` to create a new configuration.
3. Call it `rebuild federate`
4. In the `Main` tab:
   1. Under `Location` choose `Browse Workspace...` and select `federate/rebuild_federate.sh`
   2. Still in the `Main` tab under `Working Directory` choose `Browse Workspace...` and select `federate`
   {{< figure src="../images/omnetpp-ide-build-config.png" title="OMNeT++ IDE Build Configuration" numbered="true" >}}
5. In the `Build` tab __uncheck__ the box before `Build before launch`  
   {{< figure src="../images/omnetpp-ide-build-config-2.png" title="OMNeT++ IDE Build Configuration" numbered="true" >}}
6. Now you can `Apply` your changes and click on `Run`.  
7. Since you have built the project at least once, you can rebuild it again by clicking here:  
   {{< figure src="../images/omnetpp-ide-run-rebuild.png" title="Run rebuild" numbered="true" >}}

**The following video shows the above described steps:**

{{< video src="../images/omnetpp-ide-rebuild-federate.mp4" controls="yes" >}}

---

## Configure Debug Configuration

To debug the Eclipse MOSAIC OMNeT++ Federate during simulation you need to create a Debug Configuration. The following
instruction will tell you how to do that:

1. In your OMNeT++ IDE choose `Run` > `Debug Configurations...`
2. In the new window double-click on `OMNeT++ Simulation` in the left column and name the new created debug configuration `federate`.
3. In the `Executable` row check `other` and type `/federate/federate`
4. In the `Working dir` row type `/federate`
5. In the `Ini file(s)` row type `debug.ini omnetpp.ini`
6. At the end of the page click on the `More >>` link. And make sure all fields in the `Advanced` area are __empty__.
7. For `Projects to build` select `Do not build automatically before launch`
8. Now `Apply` your changes and try your configuration by clicking `Debug`

**The following images shows the final debug configuration:**

{{< figure src="../images/omnetpp-ide-debug-config.png" title="Final debug configuration" numbered="true" >}}
