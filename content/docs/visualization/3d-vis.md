---
title: 3D Visualizer
linktitle: 3D Visualizer
toc: false
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 50
menu:
  docs:
    parent: visualization
---

{{% alert extended %}}  
The 3D visualization is part of **[MOSAIC Extended](/download#overview)**.  
For further information on licenses, feel free to contact us via **[mosaic@fokus.fraunhofer.de](mailto:mosaic@fokus.fraunhofer.de)**.
{{% /alert %}}

The Eclipse MOSAIC 3D Visualization Tool is based on the PHABMACS vehicle simulator and uses the same [3D engine](https://github.com/fabmax/kool) 
and models to visualize vehicle movements and various events which occur during the simulation.
Next to the road network, which can be optionally rendered by the visualizer, the following units and
events are visualized:

* Vehicle movements coming from the traffic simulation
* Road Side Units at their defined location
* V2X-messages sent via cellular communication (indicated as green circles)
* V2X-messages sent via ITS-G5 communication (indicated as blue circles)
* V2X-messages received by vehicles (indicated as red circles)

{{< figure width="60%" src="../images/mosaic-vis-0.jpg" title="Visualize the map of the scenario." numbered="true" >}}
{{< figure width="60%" src="../images/mosaic-vis-1.jpg" title="Track vehicle movements." numbered="true" >}}
{{< figure width="60%" src="../images/mosaic-vis-2.jpg" title="See communication events." numbered="true" >}}

## Configuration

* The main configuration file for all output generators is located at `<scenarioName>/output/output_config.xml`
* A new output generator with the following configuration has to be added:

```xml
<output id="3d-visualization" enabled="true" loader="com.dcaiti.mosaic.fed.visualizer.Mosaic3dVisualizerLoader">
    <map>Barnim.osm.gz</map><!-- must be placed in <scenario>/output/assets/maps -->
    <models>
        <vehicle model="s-class.model.gz" colors="magenta,red"/>
        <vehicle model="b-class-detailed.model.gz" colors="green"/>
        <vehicle model="smart_fortwo.model.gz" colors="red,blue,yellow"/>
    </models>
    <subscriptions>
        <subscription id="VehicleRegistration" enabled="true"/>
        <subscription id="VehicleUpdates" enabled="true"/>
        <subscription id="V2xMessageReception" enabled="true"/>
        <subscription id="V2xMessageTransmission" enabled="false"/>
    </subscriptions>
</output>
```

### Map File 

The Map file (`*.osm` - OpenStreetMap) must be placed inside the path `<scenarioName>/output/assets/maps` and then referenced
in the `<map>` tag of the configuration.

### 3D Models

For each vehicle a 3D-model is chosen from a set of available models.
The visualizer includes the following models:

* Small car (Smart Fortwo): `smart_fortwo.model.gz` 
* Compact Van (Mercedes B-Class): `b-class-detailed.model.gz`
* Long Limousine (Mercedes S-Class): `s-class.model.gz`
* Truck : `truck.model.gz`


You can furthermore assign specific models to be used for specific [vehicle prototype](/docs/simulators/application_mapping#prototypes) only:

```xml
<models>
    <vehicle vehicleType="Car" model="s-class.model.gz" colors="magenta,red"/>
    <vehicle vehicleType="Truck" model="truck.model.gz" colors="green"/>
</models>
```

If no `vehicleType` is given, the model is used for any vehicle to be visualized.

Generally, it is possible to use own custom models.
These must be converted by **[us](mailto:mosaic@fokus.fraunhofer.de)** from a Jollada file before they are able to be used in the visualizer.

### Vehicle Colors

The `colors` field is a comma-separated list
of which one color is chosen randomly when a vehicle is inserted. The following colors are available:
`black`, `dark_gray`, `gray`, `light_gray`, `white`, `red`, `green`, `blue`, `yellow`, `cyan`, `magenta`, `orange`, `lime`, `light_red`, 
`light_green`, `light_blue`, `light_yellow`, `light_cyan`, `light_magenta`, `light_orange`, `dark_red`, `dark_green`, `dark_blue`, 
`dark_yellow`, `dark_cyan`, `dark_magenta`, `dark_orange`

## Visualization Window

### Pausing the Simulation

It is possible to pause the simulation by pressing <kbd>Spacebar</kbd> on your keyboard. It is important to start MOSAIC Extended
with the parameter `-w 0` when using this feature, as otherwise the visualizer would interrupt the simulation when pausing too long.

### Tracking of Vehicles

Initially, the first spawned vehicle is focused and followed in the visualizer. 
Vehicle tracking can be disabled by pressing <kbd>Esc</kbd> on your keyboard. 
Furthermore, you can double-click on any vehicle to track it.

### Simulation Speed

For better visualization it may be required to slow down simulation, since MOSAIC always tries to simulate as fast as possible.
To do so, you can start MOSAIC Extended with the parameter `-b 2` (i.e., slowing down to not faster than 2 times realtime) or any other suitable value.