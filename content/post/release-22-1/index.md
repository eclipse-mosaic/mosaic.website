---
title: 2022 Autumn Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 22.1"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/22.1"
date: "2022-10-18T00:00:00Z"
lastMod: "2022-10-19T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The autumn release of Eclipse MOSAIC is here! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 22.1 to the open source community. 
This new version extends the recently introduced perception facilities in the Application simulator and upgrades the ns-3 simulator coupling.**

You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic).

### Release Date
2022-10-19

### Changelog

```shell
[A+] Perception module is now working with viewing angles larger than 180 degrees.
[A+] Introducing perception modifiers, such as occlusion or simple error models.
[A+] Improved API of vehicle operating system; action methods (e.g., change speed) now accept nanoseconds.
[M+] Improved handling of priorities when processing time advance requests.
[M+] Allow parallel execution of federates based on same Docker image (e.g., when running simulations in parallel).
[M+] Added configuration option to configure decimal separator in file output generator.
[C+] Upgraded ns-3 federate to support ns3-36.1.
[C+] Improved default logging of OMNeT++ and ns-3 simulations to be less verbose.
[C-] Fixed a bug in OMneT++ federate using the wrong `omnetpp.ini`.
[S+] Added import of building information from OSM, e.g., for wall occlusion in perception module. 
[T+] Improved coupling of traffic simulator SUMO with vehicle simulators (e.g., PHABMACS or Carla).
[T+] Now supports SUMO 1.14.1
[T+] Released Berlin SUMO Traffic (BeST) scenario at https://github.com/mosaic-addons/best-scenario
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> fabmax](https://github.com/fabmax),
[ <i class="fab fa-github"></i> felixlutz](https://github.com/felixlutz),
[ <i class="fab fa-github"></i> fhlig](https://github.com/fhlig),
[ <i class="fab fa-github"></i> FunKuchen](https://github.com/FunKuchen),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> schwepmo](https://github.com/schwepmo),
[ <i class="fab fa-github"></i> realmaxneu](https://github.com/realmaxneu),
[ <i class="fab fa-github"></i> rprotzmann](https://github.com/rprotzmann), and
[ <i class="fab fa-github"></i> thonag](https://github.com/thonag).

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