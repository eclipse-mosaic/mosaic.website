---
title: "Workshop: Traffic Signs and SimRunner"
categories:
   - Workshop
linktitle: workshop_highway_vms
toc: false
type: workshops
draft: false
pagination_prev: perception_and_parking
pagination_next: sensor_data_generation
---

In this workshop we focus on optimizing travel times for buses on a highway transitioning to a normal road. A natural bottleneck occurs due to the merging of high car and bus flows. Variable message signs (VMS) will be utilized to manage traffic effectively.

As part of the workshop, you will learn in detail about several features of MOSAIC:

- Placement of Variable Message Signs
- Dynamic change of Variable Message Signs
- Measuring performance via the Statistics Output
- How to setup multiple parallel simulation runs
- Visualization via SUMO GUI

A major part of the workshop will teach knowledge about MOSAIC and the features required to implement the use-case scenario, as well as theoretical background knowledge about the components VMS, Statistics and Simrunner. To practice the new learnings, we will provide you with a workshop scenario for MOSAIC and several tasks the participants will solve on their own using the MOSAIC framework.

{{< figure src="images/Scenario.png" width="700px" >}}

**Task 1: Static VMS assignment**  
Participants will get familiar with the basic handling and configuration of the VMS component. By assigning different vehicle types on the respective three lanes, they will explore and optimize the scenario, following their qualitative judgement.

**Task 2: Statistics Output**  
After finding a promising lane assignment, participants will measure the impact of the VMS settings on travel times for both cars and buses. They will configure the output to collect average travel times, analyzing how the VMS influences these times.

**Task 3: Dynamic VMS assignment**  
Recognizing that traffic flows change, participants will implement dynamic lane assignments during the simulation. They will use lane area detectors to monitor traffic volume and adjust the VMS settings accordingly. Participants will create an additional lane detector and run the code to determine effective thresholds for controlling the VMS, analyzing the resulting travel times for buses.

**Task 4: SimRunner**  
Finally, participants will explore how varying car flow affects bus travel times by using the Simulation Runner tool. They will configure the scenario for both static and dynamic lane assignments and execute a series of simulations with different car flow parameters. After completing the simulations, they will plot their results to compare the performance with and without traffic control.

**Conclusion:**  
In this workshop, participants will learn how to manage traffic flow by using VMS. They will gain hands-on experience with dynamic traffic management, data analysis techniques, and simulation tools.