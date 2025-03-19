---
title: "Workshop: Sensor Data Generation"
categories:
   - Workshop
linktitle: workshop_phabmacs_sensors
toc: false
type: workshops
draft: false
pagination_prev: traffic_signs_and_simrunner
---

In this workshop, participants will get familiar with the PHABMACS simulator, inspired by the scenario of developing a driver assistance system focused on obstacle detection using LiDAR technology.

As part of the workshop, you will learn in detail about several features of MOSAIC:

- Generation of a scenario
- Generation of routes in a scenario
- Configuration of PHABMACS
- Write applications using the LiDAR API

A major part of the workshop will teach knowledge about MOSAIC and the features required to implement the use-case scenario, as well as theoretical background knowledge about Applications, PHABMACS and the LiDAR API. To practice the new learnings, we will provide you with a workshop scenario for MOSAIC and several tasks the participants will solve on their own using the MOSAIC framework.

{{< figure src="images/PointCloud.png" width="700px" >}}

**Task 1:**  
The first task involves creating the scenario folder and setting up a minimal configuration for the simulation.

**Task 2:**  
Participants will create routes by selecting start and end points and adding the route to the database. They will then configure a vehicle and assign it to the newly created route. The PHABMACS Visualizer will display the route and the vehicle following it.

**Task 3:**  
In this task, participants will configure various aspects of PHABMACS, including vehicle settings, visualization options, and GUI settings, to ensure an optimal user experience.

**Task 4:**  
Participants will add the LiDAR sensors to the scenario. This involves understanding the technology, configuring sensor properties, and assigning the LiDAR sensor to the vehicle.

**Task 5:**  
Finally, participants will create custom applications that utilize the LiDAR API. They will develop a new application class, initialize the LiDAR sensor, and implement methods to handle updates from the sensor. Participants will learn to generate log messages for new point clouds, analyze PointCloud objects, and apply point filtering to detect obstacles. They will implement functionality to initiate a lane change when a certain threshold of detected points is exceeded.

**Conclusion:**  
In this workshop, participants will gain hands-on experience with the PHABMACS simulator and the utilization of LiDAR technology, e.g. for obstacle detection. They will learn how to configure simulations, create routes, implement sensing technologies, and using that information to initiate lane changes. 