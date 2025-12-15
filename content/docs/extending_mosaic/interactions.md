---
title: Interactions in Eclipse MOSAIC
linktitle: Interactions
toc: true
type: docs
date: "2020-08-06"
draft: false
weight: 30
menu:
  docs:
    parent: extending_mosaic
---

This chapter is intended to document interactions that can be sent and received between Eclipse MOSAIC federates
in order to enable users to easily design and integrate new federates and connect them to existing
federates. Note that we use the word "interaction" whenever we describe communication between federates,
whereas we talk about a "message" primarily when concerned with V2X-communication.

All interactions are categorized by the package they are contained in, which corresponds to their main context/purpose.
The "Sent by" column is not necessarily complete for all interactions but should give an idea where interactions are used.

## mapping

All interactions in this package are concerned with adding/registering simulation-units to the simulation.
While most of the introductions are handled by the `Mapping`- or `Application`-ambassador, some of them
are handled by the ambassadors for the traffic simulators.
In this case the interactions are used to initialize the scenario.

| Interaction name              | Description                                                                                                                                                                                                                            | Sent by                     |
|:------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------|
| `AgentRegistration`           | This interaction indicates the introduction of an agent (person) unit to the simulation.                                                                                                                                               | `Mapping`                   |
| `ChargingStationRegistration` | This interaction indicates the introduction of a charging station to the simulation. All units are introduced at simulation start time, e.g. 0s.                                                                                       | `Mapping`                   |
| `RsuRegistration`             | This interaction indicates the introduction of a roadside unit to the simulation.                                                                                                                                                      | `Mapping`                   |
| `ServerRegistration`          | This interaction indicates the introduction of a server to the simulation. It holds a list of all applications.                                                                                                                        | `Mapping3`                  |
| `TmcRegistration`             | This interaction indicates the introduction of a traffic management center to the simulation. It holds a list of all applications and all induction loops and lane are detectors the TMC is responsible for.                           | `Mapping`                   |
| `TrafficLightRegistration`    | This interaction indicates the introduction of a traffic light (group) to the simulation. It holds a traffic light group, which unites all traffic light signals used for one traffic light system.                                    | `Mapping`                   |
| `TrafficSignRegistration`     | This interaction indicates the introduction of a traffic sign.                                                                                                                                                                         | `Mapping`                   |
| `VehicleRegistration`         | This interaction is sent by the vehicle mapping component to indicate the introduction of a new vehicle to the simulation. Note, this does not necessarily imply that the added vehicle is already simulated by the traffic simulator. | `Mapping`,<br>`Application` |

### mapping.advanced

Interactions in this package are still concerned with the mapping of vehicles but differ from simple simulation unit registration.

| Interaction name                   | Description                                                                                                                                                                                                                                                                                                         | Sent by               |
|:-----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------|
| `ScenarioTrafficLightRegistration` | This interaction contains the phases and their duration of each traffic light in the simulation. This interaction shall be generated by the traffic simulators at start up (e.g. SUMO)                                                                                                                              | `Sumo`,<br>`Phabmacs` |
| `ScenarioAgentRegistration`        | This interaction is used to register agents based on persons which have been added by SUMO itself from preconfigured route files.                                                                                                                                                                                   | `Sumo`                |
| `ScenarioVehicleRegistration`      | This interaction is used to register vehicles which have been added by SUMO itself from preconfigured route files.                                                                                                                                                                                                  | `Sumo`                |
| `RoutelessVehicleRegistration`     | This interaction is sent the vehicle mapping component to indicate the introduction of a new vehicle whose route is not yet known. This interaction is usually handled by the `Application`-ambassador, which calculates a route for the given vehicle and sends out an VehicleRegistration-interaction afterwards. | `Mapping`             |

## communication

This package contains interactions regarding the setup of communication-components in simulation units and interactions concerned with the
transfer of V2X-messages.

| Interaction name                     | Description                                                                                                                               | Sent by       |
|:-------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------|:--------------|
| `AdHocCommunicationConfiguration`    | This interaction is intended to be used to exchange information about the configuration of a vehicle’s ad-hoc communication facilities.   | `Application` |
| `CellularCommunicationConfiguration` | This interaction is intended to be used to exchange information about the configuration of a vehicle’s cellular communication facilities. | `Application` |

Interactions related to V2X-message transfer.

| Interaction name            | Description                                                                                                                                                                                   | Sent by                                   |
|:----------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|
| `V2xMessageAcknowledgement` | This interaction is used by a communication simulator to inform about success or failure of a packet transmission. Typically, the application simulator should subscribe to this interaction. | `Cell`                                    |
| `V2xMessageReception`       | This interaction is intended to be used to exchange information about a received V2X message.                                                                                                 | `OMNeT++`,<br>`ns-3`,<br>`SNS`,<br>`Cell` |
| `V2xFullMessageReception`   | This interaction carries the payload that represents an arbitrary V2XMessage that is supposed to be received by the receiver of this Eclipse MOSAIC Interaction.                              | `OMNeT++`,<br>`ns-3`,<br>`SNS`,<br>`Cell` |
| `V2xMessageRemoval`         | This interaction is intended to be used to exchange information about  V2X messages, that are to be deleted.                                                                                  | `Application`                             |
| `V2xMessageTransmission`    | This interaction is sent in order to trigger the transportation of a V2XMessage over a wireless network to a given geographical destination area.                                             | `Application`                             |

## vehicle

The interactions contained in this package are usually used to enable applications to forward vehicle-control to the used traffic
simulators.

| Interaction name                  | Description                                                                                                                                                                                                                                     | Sent by                       |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------|
| `VehicleDistanceSensorActivation` | This interaction is used to enable the distance sensors of a vehicle. A multitude of sensors can be enabled with one interaction given they use the same maximum lookahead distance.                                                            | `Application`                 |
| `VehicleFederateAssignment`       | This interaction is intended to be used, when more than one traffic simulator is used in a simulation. It enables the passing of information on which vehicle are passed externally. At the moment this is used by the Traffic-Sync-ambassador. | `Traffic Coupling` (Extended) |
| `VehicleLaneChange`               | This interaction initiates a lane change for the given vehicle, which should be consumed by the traffic simulators.                                                                                                                             | `Application`                 |
| `VehicleParametersChange`         | This interaction requests to update various parameters of the vehicle or its driver.                                                                                                                                                            | `Application`                 |
| `VehicleResume`                   | This interaction requests the given vehicle to continue its journey after being stopped beforehand. The interaction should be handled by traffic simulators                                                                                     | `Application`                 |
| `VehicleRouteChange`              | This interaction is used to change a route for vehicle by its id. It can be assumed that the given route id has been propagated by either a VehicleRoutesInitialization- or VehicleRouteRegistration-interaction.                               | `Application`                 |
| `VehicleRouteRegistration`        | This interaction is used to propagate a newly generated route which is not yet known. It consists of the id of the route and a list of all its edges.                                                                                           | `Application`                 |
| `VehicleSlowDown`                 | This interaction initiates the vehicle to slow down in a given interval. The request should be handled by traffic simulators. The name 'SlowDown' is inherited by Sumo and a little bit misleading, the speed can also be increased.            | `Application`                 |
| `VehicleSpeedChange`              | This interaction sets the current speed of the given vehicle. This should be handled by traffic simulators                                                                                                                                      | `Application`                 |
| `VehicleStop`                     | This interaction requests the given vehicle to stop at the given road position, it should be handled by traffic simulators.                                                                                                                     | `Application`                 |

## traffic

Interactions in this package are focused around traffic management and communication with traffic simulators.

| Interaction name                    | Description                                                                                                                                                                                                                                                                           | Sent by                       |
|:------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------|
| `AgentUpdates`                      | This interaction is used to update the position and other data of some or all agents (persons) of the simulation.                                                                                                                                                                     | `Sumo`,<br>`Agent` (Extended) |
| `CellularHandoverUpdates`           | This interaction is used by the cell ambassador to communicate handovers.                                                                                                                                                                                                             | `Cell`                        |
| `InductionLoopDetectorSubscription` | This interaction subscribes a unit to the data of an induction loop detector, usually this will be TMCs. In order to retrieve the data, traffic simulators have to be told to omit the subscribed data.                                                                               | `Application`                 |
| `LaneAreaDetectorSubscription`      | This interaction subscribes a unit to the data of a lane area detector, usually this will be TMCs. In order to retrieve the data, traffic simulators have to be told to omit the subscribed data.                                                                                     | `Application`                 |
| `LanePropertyChange`                | This interaction contains lane properties to be changed. Concretely, it sets a list of allowed and disallowed vehicle classes per lane and a new maximum speed limit that shall be changed. The changed lane properties have to be omitted to the traffic simulator to be handled.    | `Application`                 |
| `TrafficDetectorUpdates`            | This extension of {@link Interaction} combines updates of lane area and induction loop detectors.  Usually the updates are supplied by the traffic simulator and will be filtered by applications subscribed to them.                                                                 | `Sumo`                        |
| `TrafficLightStateChange`           | This interaction is intended to be used to forward a request to change the state of a simulated traffic light. This interaction can be used to implement different controlling strategies for traffic lights.                                                                         | `Application`                 |
| `TrafficLightUpdates`               | This interaction is a container for traffic light updates. It is sent by the SumoAmbassador to inform simulation units about updated traffic light states.                                                                                                                            | `Sumo`                        |
| `VehicleUpdates`                    | This interaction is used to update the position of some or all vehicles of the simulation. It consists of three lists, containing newly added vehicles, vehicles which were updated since the last simulation step, and vehicles which have been removed from the traffic simulation. | `Sumo`,<br>`Phabmacs`         |
| `VehicleTypesInitialization`        | **This interaction is required for each simulation.** It contains predefined vehicle types. Other ambassadors may cache this interaction in order to have access on vehicle types, which are later identified by their identifier.                                                    | `Mapping`                     |
| `VehicleRoutesInitialization`       | **This interaction is required for each simulation.** It contains all routes vehicles will take during the simulation. Other ambassadors may cache this interaction in order to have access on the routes, which are later identified by their identifier.                            | `Application`                 |

## electricity

All interactions contained in this package are related to electric vehicles and the surrounding infrastructure.

| Interaction name              | Description                                                                                                                                                                                                                                                 | Sent by                       |
|:------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------|
| `ChargingDenialResponse`      | This interaction is sent out by the charging station ambassador to inform the application simulator (the vehicles) when a charging station is already in use. e.g. a vehicle wants to start charging on an engaged charging station -> ChargingStartDenial. | `ChargingStation`  (Extended) |
| `ChargingStartResponse`       | This interaction is intended to be used to forward a started charging process at a ChargingSpot to the RTI.                                                                                                                                                 | `ChargingStation`  (Extended) |
| `ChargingStationUpdates`      | This extension interaction is intended to be used to forward the updated state of a ChargingStation to the RTI.                                                                                                                                             | `ChargingStation` (Extended)  |
| `ChargingStopResponse`        | This interaction is intended to be used to forward a stopped charging process at a charging station to the RTI.                                                                                                                                             | `ChargingStation` (Extended)  |
| `VehicleChargingStartRequest` | This interaction is intended to be used to forward a request from a vehicle to **start** charging its battery at a charging station to the RTI.                                                                                                             | `Application`                 |
| `VehicleChargingStopRequest`  | This interaction is intended to be used to forward a request from a vehicle to **stop** charging its battery at a charging station to the RTI.                                                                                                              | `Application`                 |
| `VehicleBatteryUpdates`       | This interaction is used to inform the applications of simulation units about changed electric information, send out by the battery ambassador.                                                                                                             | `Battery` (Extended)          |

## environment

The interactions in this package are used for handling the communication about environment sensors.

| Interaction name              | Description                                                                                                                                                          | Sent by       |
|:------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------|
| `EnvironmentSensorActivation` | This interaction is intended to be used to signal interest in sensor information for a specific simulation unit.                                                     | `Application` |
| `EnvironmentSensorUpdates`    | This interaction is intended to be used to exchange sensor data.                                                                                                     | `EventServer` |
| `GlobalEnvironmentUpdates`    | This extension of interaction contains a list of current environment events and their locations. Those events can than be used to react upon a changing environment. | `EventServer` |

## trafficsigns

Interactions in this package are used to communicate changes in variable traffic signs, e.g. changing the speed limit.

| Interaction name                     | Description                                                                                                                                                                                    | Sent by                  |
|:-------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------|
| `AbstractTrafficSignChange`          | This interaction can be sent to traffic sign ambassador in order to change a variable traffic sign. All interaction-classes, that are concerned with changing traffic signs extend this class. | `Application` (Extended) |
| `TrafficSignLaneAssignmentChange`    | This interaction can be sent to traffic sign ambassador in order to change a variable lane assignment sign.                                                                                    | `Application` (Extended) |
| `TrafficSignSpeedLimitChange`        | This interaction can be sent to traffic sign ambassador in order to change a variable speed sign.                                                                                              | `Application` (Extended) |
| `VehicleSeenTrafficSingsUpdate`      | This interaction stores a map of all traffic signs which are in sight distance of a specific vehicle and a map of all  traffic signs which became invalid for that vehicle.                    | `TrafficSign` (Extended) |
| `VehicleSignVisibilityConfiguration` | This interaction is used to configure the sight distance for a vehicle (driver), this information can for example be used to implement applications regarding traffic sign recognition.        | `Application` (Extended) |

## application

Interactions in this package are used for applications. They are used to communicate custom information/data via the RTI.

| Interaction name                       | Description                                                                                                                                                                                             | Sent by       |
|:---------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------|
| `ApplicationInteraction`               | This interaction is intended to be used in custom applications, it can be extended for simulation units to react upon different influences and can be used for intercommunication between applications. | `Application` |
| `ItefLogging`                          | This interaction is used to exchange log-tuples for the ITEF (Integrated Testing and Evaluation Framework).                                                                                             | `Application` |
| `SumoTraciRequest`                     | This interaction is used to send a byte array message to SUMO TraCI. The request will be handled by TraCI and trigger a SumoTraciResponse.                                                              | `Application` |
| `SumoTraciResponse`                    | This interaction holds the TraCI response for a SumoTraciRequest. It is sent by the SumoAmbassador and will usually be handled by the Application that sent the request.                                | `Sumo`        |
| `SumoSurroundingObjectsConfiguration ` | This interaction is sent if perception based on SUMO's internal calculation of surrounding vehicles is used.                                                                                            | `Application` |
