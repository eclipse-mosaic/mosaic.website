---
title: First Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 20.0"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/20.0"
date: "2020-10-19T00:00:00Z"
lastMod: "2020-10-19T00:00:00Z"
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

**The initial contribution is accomplished! With the autumn version, the committer team from Fraunhofer FOKUS and DCAITI is proud to release Eclipse MOSAIC 20.0 to the open source community. With the runtime infrastructure, core libraries and various implementations of simulators or couplings to existing ones, Eclipse MOSAIC includes the essential feature collection for simulation and virtual testing of connected and automated mobility solutions.**


### Release Date
2020-10-19

### Changelog

```shell
[M+] Moved main code to new public repository github-com/eclipse-mosaic
[M+] Changed license to EPL 2.0
[M+] Revised and refactored all public code.
[M+] Significantly improved and extended the documentation, including new tutorials
[M-] Replaced dependencies which are incompatible with EPL.
[M+] Major overhaul of configuration files, e.g.
     * vsimrti/vsimrti_config.xml -> scenario_config.json
     * etc/defaults.xml -> etc/runtime.json
[A+] Mapping configuration has been extended with new features (e.g. typeDistributions, parameter variations).
[A+] New API for traffic light applications
[C+] SNS supports most important Geo-Routing features for ad-hoc multihop communication
[T+] Now supports SUMO 1.7.0
```

**Changelog (Features and Bugfixes) Legend:**  
`[M]` Eclipse MOSAIC  
`[A]` Application simulator  
`[B]` Battery simulator  
`[C]` Communication simulator  
`[E]` Environment simulator  
`[N]` Navigation component  
`[S]` Scenario-convert  
`[T]` Traffic simulator  
`[+/-]` new Feature/Bugfix  
