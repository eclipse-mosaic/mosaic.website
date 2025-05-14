---
title: Run a simulation with Eclipse MOSAIC as Developer
linktitle: Setup MOSAIC in IDE
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
weight: 05
menu:
  docs:
    parent: extending_mosaic

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)

---

On this page we want to discuss how to run Eclipse MOSAIC directly from source code, and with an IDE. 
This is only relevant if you want to implement changes to MOSAIC itself. If you simply want to run different simulations, please skip this page.

### Prerequisites

You have to install the [same requirements](/docs/getting_started/#additional-software) as for the bundeled MOSAIC version, namely the Java SDK and Eclipse SUMO. Additionally you also need to install [Apache Maven](https://maven.apache.org/download.cgi).

### Get the code

Please find the MOSAIC code in the GitHub repository [here](https://github.com/eclipse-mosaic/mosaic). Let's say you would like to contribute your changes, so you fork the repository first, and read [guideline how to contribute](https://github.com/eclipse-mosaic/mosaic/blob/main/CONTRIBUTING.md).

```bash
git clone git@github.com:<github-username>/mosaic.git eclipse-mosaic
cd eclipse-mosaic
git remote add upstream git@github.com:eclipse-mosaic/mosaic.git # when using ssh
mvn clean install
```

### From the Command-Line-Interface

The entry class `MosaicStarter` has a lot of dependencies which need to be referenced during startup, so we recommend that you use the package functionality of maven and then run the bundle. Everything else would be overly complicated. Also, we don't build "fat jars" so you won't be able to run any jar directly. The following is almost the same procedure as presented in the non-developer guides, just that you build the bundle locally.

- Run `mvn clean install`
- Unzip one of the `bundle/target/eclipse-mosaic-{{< version of="mosaic_snapshot" >}}.zip/tar` archives to a destination of your choice
- Run the `mosaic.sh/bat` files, which will do all the magic for you (have a look inside if you ask yourself if there's a more direct way to start from console).

### Use IntelliJ IDEA

- Start IntelliJ IDEA and choose `Open or Import` (see Figure 1).
- Choose the cloned `eclipse-mosaic` repository
- For IntelliJ there's already a [pre-defined run configuration](https://github.com/eclipse-mosaic/mosaic/blob/main/rti/mosaic-starter/MosaicStarter.run.xml) in the repo which should work right away when you run it (see Figure 2).

Good to know: 
- When changing any `pom.xml` you need to: `Rightclick on root-module > Maven > Reload project`

{{< figure src="images/open-project.png" title="Open the project" numbered="true" width="300">}}
{{< figure src="images/run.png" title="Run MosaicStarter" numbered="true" width="400" >}}

### File locations

Given that you execute the code from source, some important file locations change:

| Bundled                   | From Source                             |
|:--------------------------|:----------------------------------------|
| `<mosaic-root>/scenarios` | `<mosaic-root>/scenarios/bundle`        |
| `<mosaic-root>/etc`       | `<mosaic-root>/rti/mosaic-starter/etc`  |
| `<mosaic-root>/logs`      | `<mosaic-root>/rti/mosaic-starter/logs` |

### Combine Mosaic (Windows IntelliJ IDE) with ns3 (WSL)

- In your WSL environment, download the `eclipse-mosaic-{{< version of="mosaic_snapshot" >}}.zip/tar` and install ns-3 and federate (
  using `ns3_installer.sh`).
- Copy the resulting `bin` folder (with the fully compiled ns3) into the `eclipse-mosaic/rti/mosaic-starter` folder
- Now it should behave as if you run everything from WSL