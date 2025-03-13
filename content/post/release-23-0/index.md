---
title: 2023 Spring Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 23.0"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/23.0"
date: "2023-04-20T00:00:00Z"
lastMod: "2023-04-20T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The autumn release of Eclipse MOSAIC is here! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 23.0 to the open source community. 
This new version improves the recently introduced perception facilities in the Application simulator and brings better handling of the traffic simulation SUMO.**

You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic).

### Release Date
2023-04-20

### Changelog

```shell
[A+] Perceived objects provide dimension information (length, width, height).
[A+] The perception module can now detect traffic lights and their current states.
[A+] Vehicles and other units are finally able to send V2X messages during their shutdown procedure.
[A-] Resolved issues in perception module, e.g., when multiple apps used perception modifiers simultaneously.
[M+] The RTI now detects the OS type of the local host by itself.
[M-] Fixed minor bugs in RTI, Logging, and JSON-Scheme files.
[S+] scenario-convert is now able to import building information to an existing scenario database.
[T+] Improved handling of vehicles which are teleported by SUMO.
[T+] Several data related to trains can now be read from SUMO.
[T+] Now supports SUMO 1.16.0
[X+] Introduced OpenDRIVE support for PHABMACS (Extended).
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> fhlig](https://github.com/fhlig),
[ <i class="fab fa-github"></i> FunKuchen](https://github.com/FunKuchen),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> schwepmo](https://github.com/schwepmo),
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