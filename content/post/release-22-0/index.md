---
title: 2022 Spring Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 22.0"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/22.0"
date: "2022-05-04T00:00:00Z"
lastMod: "2022-05-04T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The spring release of Eclipse MOSAIC has arrived! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 22.0 to the open source community. 
This new version introduces perception facilities to the Application simulator and improved the integration of the traffic simulator SUMO.**

You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic).

### Release Date
2022-05-04

### Changelog

```shell
[A+] A perception module has been added. Vehicles can now perceive other vehicles in their field of view.
[A+] Adjusted tutorial application WeatherServerApp to use server entity.
[A+] Improved map matching of start and end points for routing.
[A-] Fixed a bug in payload deserialization.
[M+] Added Quad-tree and Grid index for fast search of surrounding entities.
[M+] Allow configuration of a connection id as departure position of a vehicle.
[M-] Refactored and unified matrix implementations in mosaic-utils.
[T+] Improved LibSumo coupling interface.
[T+] Enhanced interface to SUMO to use context subscriptions in certain situations.
[T+] Now supports SUMO 1.13.0
[T-] Fixed wrong position problem of parked vehicles.
[X+] Introduced new physics engine in PHABMACS based on PhysX (Extended).
```

:star: A huge thanks to all contributors who participated in this release:
[ <i class="fab fa-github"></i> fabmax](https://github.com/fabmax),
[ <i class="fab fa-github"></i> felixlutz](https://github.com/felixlutz),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> ninabohm](https://github.com/ninabohm),
[ <i class="fab fa-github"></i> schwepmo](https://github.com/schwepmo), and 
[ <i class="fab fa-github"></i> rprotzmann](https://github.com/rprotzmann)

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