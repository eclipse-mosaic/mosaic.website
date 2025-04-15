---
title: 2025 Spring Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 25.0"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/24.1"
date: "2025-04-14T00:00:00Z"
lastMod: "2025-04-15T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The spring release of Eclipse MOSAIC is here! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 25.0 to the open source community.
This new version comes with an improved Application API, new components towards multi-modality, an extended 2D visualization, as well as usability improvements for the 
communication simulators OMNeT++ and ns-3 .**

{{% alert note %}}
MOSAIC 25.0 now requires at least **Java 17 Runtime Environment** to be executed. For more details, see [Getting Started](/tutorials/getting_started/).
{{% /alert %}}

:rocket: You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic). 

### Release Date
2025-04-15

### Changelog

```shell
* [M+] MOSAIC now requires at least Java 17 Runtime Environment to be executed.
* [M+] Extended 2D web visualizer to visualize other vehicle classes, such as buses or bicycles.
* [M+] Provide possibility to run OMNeT++ or ns-3 federate via Windows Subsystem for Linux (WSL). 
* [M-] Exit simulation if the output configuration is erroneous.
* [A+] Overhaul of API for building routing information for sending V2X messages (cellular and ad-hoc).
* [A+] Overhaul of API to access sensor information, such as environmental events or LiDAR point clouds.
* [A+] Introducing a new routing module for calculating public transport routes based on GTFS data.
* [S+] Added new option --list-routes to Scenario-Convert to print all routes stored in the scenario database.
* [T+] Now supports SUMO 1.22.0
* [X+] Provide a method for electric vehicles applications to search charging stations within a certain area (Extended).
* [X+] New Agent Simulator allows to model multi-modal journeys for individual agents (Extended).
* [X-] Improved route compatibility in PHABMACS (Extended). 
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> FunKuchen](https://github.com/FunKuchen),
[ <i class="fab fa-github"></i> hoelger](https://github.com/hoelger),
[ <i class="fab fa-github"></i> iwillitried ](https://github.com/iwillitried ),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> schwepmo](https://github.com/schwepmo), and
[ <i class="fab fa-github"></i> rprotzmann](https://github.com/rprotzmann).

---

> _Changelog Legend_
>   
> `[M]` _Eclipse MOSAIC_\
> `[X]` _MOSAIC Extended_\
> `[A]` _Application simulator_\
> `[C]` _Communication simulator_\
> `[E]` _Environment simulator_\
> `[N]` _Navigation component_\
> `[S]` _Scenario-convert_\
> `[T]` _Traffic simulator_\
> `[+/-]` _new Feature/Bugfix_