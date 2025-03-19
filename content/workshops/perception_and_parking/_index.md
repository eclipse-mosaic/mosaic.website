---
title: "Workshop: Perception and Parking Vehicles"
categories:
   - Workshop
linktitle: workshop_parking_observer
toc: false
type: workshops
draft: false
pagination_prev: modelling_mobility
pagination_next: traffic_signs_and_simrunner
---

In this workshop we address the implementation of a parking space monitoring use-case, which utilizes civilian vehicles equipped with sensors and antennas. In a pre-defined simulation scenario, vehicles are driving along the TU Berlin campus at Ernst-Reuter-Platz and use their perception devices to
identify other parking vehicles. A centralized server receives the sensor data and visualizes it.

As part of the workshop, you will learn in detail about several features of MOSAIC:

- Perception of surrounding traffic objects including realistic perception models
- Placement of parking vehicles
- Exchange of sensor data via communication over a cellular network
- Visualization of objects in the MOSAIC 3D Visualizer

A major part of the workshop will teach knowledge about MOSAIC and the features required to implement the use-case scenario, as well as theoretical background knowledge about perception and communication. To practice the new learnings, we will provide you with a workshop scenario for MOSAIC and several tasks the participants will solve on their own using the MOSAIC framework.

{{< figure src="images/parking_vehicles.png" width="500px" >}}

**Task 1: Communication**  
The first step involves enabling communication between the vehicles and the server. The vehicles can now transmit their positions, allowing the server to visualize them as bounding boxes in the environment. This foundational communication will be expanded in the next task to include information about other vehicles.

**Task 2: Perception**  
In this task, the focus shifts to enhancing the vehicles' capabilities to detect other vehicles in their vicinity. As a result, the ego vehicles can now send both their own positions and the positions of other detected vehicles to the server, which visualizes this information. However, the system currently only reports moving vehicles.

**Task 3: Parking Vehicles**  
With the communication and perception functions established, parking vehicles are introduced into the simulation. This addition aims to improve the monitoring of available parking spaces. The data collected by the equipped vehicles will help identify free spots, placing Berlin at the forefront of modern traffic management.

**Task 4: Occlusion Modifiers**  
Finally, the workshop aims to increase realism in the parking space monitoring system. By introducing occlusion factors and refining communication methods, the simulation becomes more authentic. While this results in less perfect data and occasional message loss, it ultimately leads to a more realistic model of a parking space monitoring system.

**Conclusion:**\
In this workshop, participants will learn about using communication and perception in simulation development, as well as techniques for visualizing vehicle data. Participants will also gain insights into simulating realistic scenarios, including the placement of parking vehicles and implementing challenges like occlusion. This hands-on experience will equip them with the skills to contribute to innovative traffic management solutions in urban settings.