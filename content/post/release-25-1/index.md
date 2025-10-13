---
title: 2025 Autumn Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 25.1"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/25.1"
date: "2025-10-10T00:00:00Z"
lastMod: "2025-10-13T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The autumn release of Eclipse MOSAIC is here! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 25.1 to the open source community.
The new version finally introduces detailed full-stack simulation of cellular communication in ns-3, starting with the LTE C-V2X stack.
Furthermore, it is now possible to integrate persons from SUMO into MOSAIC simulations and equip them with cellular communication devices.**

{{% alert note %}}
MOSAIC 25.1 requires at least **Java 17 Runtime Environment** to be executed. For more details, see [Getting Started](/tutorials/getting_started/).
{{% /alert %}}

:rocket: You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic). 

### Release Date
2025-10-13

### Changelog

```shell
* [C+] Introduced simulation of cellular communication using ns-3 federate (LTE stack).
* [C+] Upgraded OMNeT++ federate to OMNeT++ 6.1
* [C+] Exchange cellular messages among persons by using agent applications and mosaic-cell simulator. 
* [A+] Added possibility to map agent applications to SUMO persons via mapping configuration.
* [A+] Major overhaul of environment event API.
* [M+] Perception models are now available as separate library.
* [M-] Startup procedure and logging of docker-based federates is now more robust. 
* [S+] Completely new command line interface for scenario-convert tool (see `--help` or website).
* [T+] Now supports SUMO 1.24.0
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> hoelger](https://github.com/hoelger),
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