---
title: 2024 Spring Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 24.0"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/24.0"
date: "2024-03-25T00:00:00Z"
lastMod: "2024-03-25T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The spring release of Eclipse MOSAIC has arrived! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 24.0 to the open source community. 
This new version comes with an updated routing library and improved simulation of cellular communication.**

:rocket: You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic). 

### Release Date
2024-03-25

### Changelog

```shell
* [A-] Renamed API method getPayLoad() to getPayload(). Needs migration of all V2xMessage extensions.
* [A+] Upgraded the integrated routing engine GraphHopper, enabling improved calculation of alternative routes.
* [A+] Route calculation and handling is much more robust now.
* [C+] Cellular simulator now models headers on top of payloads based on underlying protocols.
* [M-] Upgraded several dependencies to most recent versions.
* [T+] Lane change mode OFF now completely disables lane-changing. New mode `FOLLOW_ROUTE` disables lane changing except for route following purposes.
* [T-] Vehicle ids from existing SUMO scenarios can now be named using UTF-8 characters, such as Arabic, Chinese, Hebrew, and other.
* [T+] Now support SUMO 1.19.0
* [X+] New parking ambassador allows to put standing vehicles into the world which are synchronized with other simulators, such as SUMO (Extended).
* [X+] Major upgrade of visualization in Phabmacs and MOSAIC 3D visualizer by using new graphics engine KOOL (Extended).
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> FunKuchen](https://github.com/FunKuchen),
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