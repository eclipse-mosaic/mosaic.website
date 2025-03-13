---
title: Testing mobility scenarios with the Open-Source simulation environment Eclipse MOSAIC
categories:
  - Simulation
  - Communication
  - PHABMACS
summary: On the occasion of EclipseCon 2020, Fraunhofer FOKUS launches its simulation environment Eclipse MOSAIC. This solution is based on VSimRTI (Vehicle-2-X Simulation Runtime Infrastructure), which has been developed over the last 15 years in close cooperation with the DCAITI of the TU Berlin and has already been used by more than 600 partners to test mobility services and traffic scenarios. Eclipse MOSAIC is now partially available as open-source.
tags: []
date: "2020-10-16T00:00:00Z"
lastMod: "2020-10-16T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: "The simulation environment Eclipse MOSAIC is now available as open source. Copyright: Fraunhofer FOKUS"
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
**On the occasion of EclipseCon 2020, Fraunhofer FOKUS launches its simulation environment Eclipse MOSAIC. This solution is based on VSimRTI (Vehicle-2-X Simulation Runtime Infrastructure), which has been developed over the last 15 years in close cooperation with the DCAITI of the TU Berlin and has already been used by more than 600 partners to test mobility services and traffic scenarios. Eclipse MOSAIC is now partially available as open-source.**

Whether dynamic lane assignment or traffic light phase assistant, new mobility services are designed to increase safety, efficiency, comfort, and facilitate environmentally friendly transport. The Eclipse MOSAIC simulation environment allows to explore how this can be achieved, before the services are tested in field trials on the road. Eclipse MOSAIC can also be used for testing driver assistance systems and to optimize the entire traffic.

## Flexible coupling of simulators

Eclipse MOSAIC integrates, depending on the simulation scenario, different aspects like individual building blocks into a holistic system, e.g., traffic congestion, battery charging of electric cars, or communication between other road users and a central cloud. The level of detail for individual aspects is variable: from a rough mobility scenario for an entire city to detailed individual driving maneuvers.

The open-source version of Eclipse MOSAIC already includes several simulators, e.g., Eclipse SUMO for traffic and OMNeT++ and ns-3 for communication. Further simulators can be coupled, e.g., Fraunhofer FOKUS offers the simulator PHABMACS for the realistic modeling of autonomous vehicles.



In addition to the simulator coupling, Eclipse MOSAIC manages the following tasks:
- Federation: Individual simulators are interchangeable within a scenario.
- Interaction: Information from one simulator is also taken into account by others.
- Time: All simulators run synchronously.

Additionally, Eclipse MOSAIC offers several tools for evaluation and visualization of the results, which are also included in the open-source package.

In the recently completed EU project INFRAMIX, Eclipse MOSAIC was used to test scenarios for the future road that allow mixed traffic between conventional and automated vehicles.

Fraunhofer FOKUS has been a strategic development member of the Eclipse Foundation since May of this year and works in close cooperation with the partners of the working groups OpenMobility and openADx (Open Source for Autonomous Driving).

Further information about Eclipse MOSAIC:
https://www.eclipse.dev/mosaic
https://github.com/eclipse-mosaic/mosaic

Further information about INFRAMIX:
https://www.fokus.fraunhofer.de/de/fokus/news/inframix-projekt_2020_08

Further information about EclipseCon:
https://www.eclipsecon.org/2020
