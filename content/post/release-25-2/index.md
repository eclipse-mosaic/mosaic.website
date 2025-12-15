---
title: 2025 Christmas Release of Eclipse MOSAIC
categories:
  - Release
release:
  name: "Eclipse MOSAIC 25.1"
  github_url: "https://github.com/eclipse-mosaic/mosaic/releases/tag/25.1"
date: "2025-12-15T00:00:00Z"
lastMod: "2025-12-15T00:00:00Z"
math: false
diagram: true
featured: false
draft: false

image:
  placement: 2
  focal_point: ""
  preview_only: false
---

**The special Christmas release of Eclipse MOSAIC is here! 🎄 The committer team from Fraunhofer FOKUS and DCAITI is proud to present Eclipse MOSAIC 25.2 to the open source community.
The release brings built-in support for your DRT (Demand Responsive Traffic) simulation needs. Additionally, if you were building on large-scale ns3 simulation, our new preemptive simulation approach
should speed up things significantly.**

:rocket: You can find the new version in our [Download section](/download), and in our [GitHub repository](https://github.com/eclipse-mosaic/mosaic). 

### Release Date
2025-12-15

### Changelog

```markdown
- [A+] Introduce support for modeling demand responsive traffic (e.g., ride pooling, taxis) using SUMO
- [T+] Add support for integrating SUMO's taxi device API via TraCI/libsumo
- [M+] Introduce concept of preemptive execution in the core RTI (see upcoming publication)
- [C+] Improve performance of ns3 federate by utilizing preemptive execution
- [T+] Now supports SUMO 1.25.0
```

:star: A huge thanks to all users who contributed to this release:
[ <i class="fab fa-github"></i> Jacenty00](https://github.com/Jacenty00),
[ <i class="fab fa-github"></i> gogokotsev00](https://github.com/gogokotsev00),
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