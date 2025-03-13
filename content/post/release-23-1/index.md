---
title: 2023 Autumn Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 23.1"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/23.1"
date: "2023-10-06T00:00:00Z"
lastMod: "2023-10-06T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The autumn release of Eclipse MOSAIC has arrived! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 23.1 to the open source community. 
This new version brings the perception facilities in the Application simulator to a stable state and comes with improvements across several integrated MOSAIC models.**

{{% alert note %}}
MOSAIC 23.1 now requires at least **Java 11 Runtime Environment** to be executed. For more details, see [Getting Started](/tutorials/getting_started/).
{{% /alert %}}

:rocket: You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic). 

### Release Date
2023-10-06

### Changelog

```shell
[M+] MOSAIC now requires at least Java 11 Runtime Environment to be executed.
[M+] Improved MOSAIC Tutorials on eclipse.dev/mosaic and related applications.
[A+] Perception Module leaves experimental phase and is now available per default.
[A+] New perception modifiers model occlusion based on bounding boxes, and errors on heading and dimension of perceived objects.
[A+] The application API was extended to get access to the unit's logging directory (e.g. for data export).
[M+] Mapping of vehicle applications now follows a stochastic distribution per default instead of a repeating pattern (configurable).
[M-] Fixed that vehicle deceleration values were omitted in specific configuration setups.
[S+] Import of SUMO net files in Scenario-Convert supports almost any projection now (only UTM was supported). 
[S-] Fixed database type affinities to make scenario-database more robust.
[T+] Vehicles defined in integrated SUMO scenarios can now be mapped with complex application distributions.
[T+] Now supports SUMO 1.18.0
[X+] Path handling in Simulation-Runner is more robust now.
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> FunKuchen](https://github.com/FunKuchen),
[ <i class="fab fa-github"></i> iwillitried ](https://github.com/iwillitried ),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> philipstr01](https://github.com/philipstr01),
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