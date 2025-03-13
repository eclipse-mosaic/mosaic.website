---
title: 2021 Autumn Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 21.1"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/21.1"
date: "2021-10-07T00:00:00Z"
lastMod: "2021-10-07T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**A new release of Eclipse MOSAIC is here! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 21.1 to the open source community. 
This new version focuses on a better usage of communication simulators SNS, OMNeT++, and ns-3, and allows to use a much faster integration of the traffic simulator SUMO.**

You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic).

### Release Date
2021-10-07

### Changelog

```shell
[A+] Server units are now able to access the central navigation component for routing purposes.
[A+] The stop mode has been revised, allowing vehicles to park in parking areas (SUMO).
[M-] WebVisualizer now removes vehicles correctly and shows V2X indicators longer.
[M-] Fixed a bug in matrix mappers configuration in mapping.
[C+] Upgraded ns-3 federate to support ns3-34.
[C+] Major improvement of logging for SNS, OMNeT++, and ns-3.
[C-] Fixed a bug in polygon intersection test used by reachability check in mosaic-cell.
[S+] Improved Scenario-convert for faster and more reliable import of SUMO net files.
[T+] You can now use LibSumo as an alternative to TraCI (experimental).
[T+] Now supports SUMO 1.10.0
[X+] Major overhaul of battery and charging station simulation (Extended).
[X+] Added new consumption model for Li-Ion based batteries (Extended).
```

:star: A huge thanks to all contributors who participated in this release:
[ <i class="fab fa-github"></i> felixlutz](https://github.com/felixlutz),
[ <i class="fab fa-github"></i> realmaxneu](https://github.com/realmaxneu),
[ <i class="fab fa-github"></i> fhilg](https://github.com/fhilg),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> paguos](https://github.com/paguos),
[ <i class="fab fa-github"></i> schwepmo](https://github.com/schwepmo), and 
[ <i class="fab fa-github"></i> vogtva](https://github.com/vogtfa)

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