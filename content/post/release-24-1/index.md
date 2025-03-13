---
title: 2024 Autumn Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 24.1"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/24.1"
date: "2024-09-06T00:00:00Z"
lastMod: "2024-09-06T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The autumn release of Eclipse MOSAIC is here! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 24.1 to the open source community.
This release concentrates less on new features, but on improving documentation and tutorials.**

:rocket: You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic). 

### Release Date
2024-09-06

### Changelog

```shell
* [M+] The file output generator is now able to print fields of the V2X message related to a V2xMessageReception interaction.
* [M+] Geomath now provides a new class representing point clouds.
* [A+] V2X messages can be duplicated for re-transmissions.
+ [A-] Searching for road positions is now more accurate for overlapping edges.
* [S+] Scenario-Convert is now able to export the database to GeoJSON files (removed shapefile export).
* [T+] Now supports SUMO 1.20.0
* [X-] Major clean-up of extended bundle and dependencies (Extended).
* [X-] Improved synchronization of timing between PHABMACS and MOSAIC (Extended).
* [X+] PHABMACS and MOSAIC 3D Visualizer now use an own 3D model format based on protobuf (Extended).
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