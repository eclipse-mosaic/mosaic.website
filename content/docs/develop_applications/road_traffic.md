---
title: Scenario Database
linktitle: Scenario Database
toc: true
type: docs
date: "2019-08-06"
draft: false
weight: 80
menu:
  docs:
    parent: develop_applications
    weight: 80
---

Each scenario to be simulated with Eclipse MOSAIC requires a database which contains information about the road infrastructure and routes the 
vehicles drive on. This information is used by various federates. For example, the SUMO federate needs to know initial routes for vehicles, 
and the [Application Simulator](/docs/simulators/application_simulator) requires detailed information about the road 
infrastructure to provide applications with methods for route calculation. For this purpose, an embedded SQLite database is 
used which is placed in the `application` folder of the scenario. This database consists of the following tables:

### Database tables

| Database Name                           | Description                                                                                                                                                                                                                                                              |
|:----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Node`                                  | Contains all Nodes of the road network such as junctions and nodes describing the geometry of a road. Each node is identified by an unique ID (long).(refer to {{< target-blank "http://wiki.openstreetmap.org/wiki/Node" "http://wiki.openstreetmap.org/wiki/Node" >}}) |
| `Way`                                   | Provides various properties for each way of the road network.(refer to {{< target-blank "http://wiki.openstreetmap.org/wiki/Way" "http://wiki.openstreetmap.org/wiki/Way" >}}                                                                                            |
| `WayConsistsOf`                         | Provides a list of nodes for each way of the road network.                                                                                                                                                                                                               |
| `Connection`                            | Contains a list of all connections of the road network including the way it originally is part of. Each connection describes an *directed* edge between two junctions in the road network.                                                                               |
| `ConnectionConsistsOf`                  | Provides a list of nodes each connection consists of.                                                                                                                                                                                                                    |
| `Restriction`                           | Provides information about turn restrictions. Each turn restriction is described by a from-connection, a via-node, and a to-connection. This data is used for route calculation purposes.                                                                                |
| `Roundabouts`<br>`RoundaboutConsistsOf` | Contains information about connections belonging to roundabouts.                                                                                                                                                                                                         |
| `Route`                                 | Contains a list of all routes known for the simulation scenario. All routes referenced in the `Mapping` configuration must be presentin this table.                                                                                                                      |
| `Building` <br>`BuildingCorner`         | Provides information about buildings alongside the road network, e.g. for visualization purposes, [occlusion models](/docs/develop_applications/perception), or sophisticated communication simulation models.                                                           |
| `Properties`                            | Contains the version of the Eclipse MOSAIC installation which was initially used to create the database.                                                                                                                                                                 |

### Road network model

This section describes the model of the road network used by various components of Eclipse MOSAIC. In the next figure various nodes 
and connections can be seen. A `node` is either a junction or describes the geometry of a road. A `connection` is a directed edge 
between two junction nodes. That also means, that two separate connections exists for a road segment which can be traversed in both 
directions. Each connection consists of at least two nodes (start and end junction node). Between those nodes, other nodes can exist 
which describe the curvature of the road segment. Furthermore, each connection has a reference to its originating `way`, which may 
consist of various connections. A way contains further properties, such as the maximum speed or the type of the road.

{{< figure src="../images/node-connections.jpeg" title="Nodes and connections of the road network" numbered="true" width="50%" >}}

Nodes and ways are identified by unique IDs derived from the base OSM network file. Connections, however, are not part of the OSM standard 
and their identifiers are generated during the import. Each connection ID consists of three parts (using the string pattern `aaa_bbb_ccc`):

* `aaa` - ID of the originating way
* `bbb` - ID of the node the connection starts at.
* `ccc` - ID of the node the connection ends in.

{{< figure src="../images/connections.jpeg" title="ID of connection in road network" numbered="true" width="50%" >}} 