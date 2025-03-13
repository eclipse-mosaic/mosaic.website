---
title: 2021 Spring Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 21.0"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/21.0"
date: "2021-03-10T00:00:00Z"
lastMod: "2021-03-10T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The spring release has arrived! The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 21.0 to the open source community. 
This new version focuses on a much better integration of SUMO configurations, and introduces a new Server entity to the Application Simulator.**

You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic).

Please note our [Migration Guide](#migration-guide) below when updating Eclipse MOSAIC.

### Release Date
2021-03-10

### Changelog

```shell
[T+] It is now possible to map applications on vehicles which are defined in SUMO configurations. 
[T+] Simplified the internal road network model for a better integration of existing SUMO scenarios.
[C+] Implemented much faster reachability check in SNS.
[A+] Added the possibility to map an application on all existing traffic lights at once.   
[A+] New simulation entity for Server applications. 
[M-] Fixes a minor bug in the contains check of polygons
[M+] Added complete documentation for most configuration files to the website.
[M+] Added a new tutorial showcasing the integration of existing SUMO configurations.
[T+] Now supports SUMO 1.8.0
```

:star: A huge thanks to all contributors who participated in this release:
[ <i class="fab fa-github"></i> fabmax](https://github.com/fabmax),
[ <i class="fab fa-github"></i> kschrab](https://github.com/kschrab),
[ <i class="fab fa-github"></i> paguos](https://github.com/paguos),
[ <i class="fab fa-github"></i> schwepmo](https://github.com/schwepmo), and 
[ <i class="fab fa-github"></i> vogtva](https://github.com/vogtfa)

### Migration Guide  

With the improved integration of SUMO scenarios it is now possible to create a MOSAIC scenario based on any
existing SUMO scenario (`*.sumocfg`, `*.net.xml`, and `*.rou.xml`). To achieve, we had to adjust our 
road network model in a way that it matches better the network representation of SUMO. 

**This adjustment, however, affects all existing MOSAIC scenarios.**

The following steps should be followed if you want to migrate your already existing MOSAIC scenario to the latest version:

1. Download the newest version of [scenario-convert](https://www.dcaiti.tu-berlin.de/research/simulation/download).
2. Run `scenario-convert --update -d path/to/database.db` to update the database file of your scenario.
3. Run `scenario-convert --db2sumo -d path/to/database.db` to generate a new SUMO network.
4. Move the generated `*.net.xml` file to the `sumo` directory of your scenario and replace the existing one with it.

If you need further advice, please head over to our all new [discussion board](https://github.com/eclipse-mosaic/mosaic/discussions). 


---

> _Changelog Legend_
>   
> `[M]` _Eclipse MOSAIC_\
> `[A]` _Application simulator_\
> `[B]` _Battery simulator_\
> `[C]` _Communication simulator_\
> `[E]` _Environment simulator_\
> `[N]` _Navigation component_\
> `[S]` _Scenario-convert_\
> `[T]` _Traffic simulator_\
> `[+/-]` _new Feature/Bugfix_