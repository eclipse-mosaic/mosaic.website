---
title: "Barnim: Develop Applications"
categories:
 - Tutorial
linktitle: barnim_develop_applications
toc: false
type: tutorials
draft: false
pagination_prev: barnim_simulation_basics
pagination_next: create_new_scenario
---

{{% alert note %}}
All files you need for this tutorial are included in the Eclipse MOSAIC zip file:  
**[Download Eclipse MOSAIC](/download)**
{{% /alert %}}

The **Barnim: Develop Applications** tutorial extends the preceding tutorial and focuses on the implementation of applications
used in the Barnim scenario. In this section, the actual source code used to create the applications is explained, 
 giving a first impression on the API of the **Eclipse MOSAIC Application** simulator.

{{% alert learning_objectives %}}
After completing this tutorial you will be able to:

* Develop Applications for simulation entities.
* Transmit and receive V2X-messages periodically or on request depending on the network type and communication mode (
e.g. topologically-scoped unicast, geographically-scoped broadcast, etc.). 
* Calculate alternative routes to the destination to circumnavigate a road obstacle.
{{% /alert %}}

As mentioned in [Barnim: Simulation Basics](/tutorials/barnim_simulation_basics#overview-of-applications), the Barnim
tutorial is structured in four applications.
1. [`SlowDownApp`](#slowdownapp) - Models driver behavior by slowing the vehicle down as soon as it enters an affected road segment.
2. [`WeatherWarningApp`](#weatherwarningapp) - Detects bad road conditions, broadcasts information via ad-hoc communication, circumnavigates areas with bad road conditions.
3. [`WeatherWarningAppCell`](#weatherwarningappcell) - Uses cellular communication and receives messages from `WeatherServerApp`. Circumnavigates areas with bad road conditions.
4. [`WeatherServerApp`](#weatherserverapp) - Broadcasts information about bad road conditions via cellular communication.

## General application structure

Applications can be mapped to various simulation entities, such as vehicles, servers, or traffic lights. The most basic structure for an application mapped onto a vehicle looks like the following:

```java  
public class ExampleApp extends AbstractApplication<VehicleOperatingSystem> implements VehicleApplication {   
 
    @Override 
    public void onStartup() { 
    } 
    
    @Override 
    public void onVehicleUpdated(VehicleData previousVehicleData, VehicleData updatedVehicleData) {  
    }
     
    @Override  
    public void processEvent(Event event){     
    }   
      
     @Override 
     public void onShutdown() {
     }   
}  
```

`onVehicleUpdated` needs to be defined and overwritten in order to successfully implement the interface `VehicleApplication`.
`processEvent` handles all events that get called to the application specific event manager, like this: 
```java  
getOs().getEventManager().addEvent(new Event(getOs().getSimulationTime(), this));  
```
Some examples of events are shown below, but a more detailed description can be found in the 
[documentation](/docs/develop_applications/event_scheduling).


## SlowDownApp

The [`SlowDownApp`](https://github.com/eclipse-mosaic/mosaic/blob/main/app/tutorials/weather-warning/src/main/java/org/eclipse/mosaic/app/tutorial/SlowDownApp.java) induces a speed reduction as soon as the on-board sensors detect hazardous conditions.
To detect the change in the on-board sensors, the state of the sensors have to be queried whenever the
vehicle has moved. This is achieved by implementing the `onVehicleUpdated()` method which is called
whenever the traffic simulator executed one simulation step.

In this specific implementation, the speed of the vehicle is reduced to *25 km/h* within the entire hazardous area.
After leaving the hazardous area, the vehicles returns to their original speed again:

```java

private final static float SPEED = 25 / 3.6f;

private boolean inHazardousArea = false;

@Override
public void onStartup() {
    getOs().getBasicSensorModule().enable();
}

@Override
public void onVehicleUpdated(VehicleData previousVehicleData, VehicleData updatedVehicleData) {
    
    SensorType[] types = SensorType.values();
    int strength = 0;
    
    for (SensorType currentType : types) {
        strength = getOs().getBasicSensorModule().getStrengthOf(currentType);
    
        if (strength > 0) {
            break;
        }
    }
    
    if (strength > 0 && !inHazardousArea) {
        getOs().changeSpeedWithInterval(SPEED, 5000);
        inHazardousArea = true;
    }
    
    if (strength == 0 && inHazardousArea) {
        getOs().resetSpeed();
        inHazardousArea = false;
    }
}
```

## WeatherWarningApp

The [`WeatherWarningApp`](https://github.com/eclipse-mosaic/mosaic/blob/main/app/tutorials/weather-warning/src/main/java/org/eclipse/mosaic/app/tutorial/WeatherWarningApp.java) application illustrates the vehicles and their behaviour in particular with regard to the 
detecting hazardous area, the receiving and sending messages. We will also cover how alternative routes 
will be calculated and how vehicles change their route in order to circumnavigate the affected road area.

The simulating vehicles can be operated as Cellular or Ad-hoc equipped. Accordingly, the kind of used communication 
network will be activated in the `onStartup()` method as following:

```java
public void onStartup() {
    getLog().infoSimTime(this, "Initialize application");
    if (useCellNetwork()) {
        getOs().getCellModule().enable();
        getLog().infoSimTime(this, "Activated Cell Module");
    } else {
        getOs().getAdHocModule().enable(new AdHocModuleConfiguration()
            .addRadio()
                .channel(AdHocChannel.CCH)
                .power(50)
                .create());
        getLog().infoSimTime(this, "Activated AdHoc Module");
    }
    
    getOs().requestVehicleParametersUpdate()
            .changeColor(Color.RED)
            .apply();
}
```

In case the sensor detects an environmental hazard the vehicle sends out a DEN-message to warn
other vehicles. In the `reactOnEnvironmentData()` method, the sending is handled with 
regard to used communication network. For the `WeatherWarningApp`,  ITS-G5 communication is used 
to inform all vehicles in reach. In case of the `WeatherWarningAppCell`, the environment event is
sent as a DEN-message directly to the server entity which runs the `EmergencyServerApp`, addressed 
by the name of the unit (i.e., `server_0`). 

```java
private void reactOnEnvironmentData(SensorType type, int strength) {

    GeoPoint vehiclePosition = getOs().getPosition();
    
    String roadId = getOs().getVehicleInfo().getRoadPosition().getConnection().getId();
    
    // reach all vehicles in a 3000m radius
    GeoCircle dest = new GeoCircle(vehiclePosition, 3000);
    
    MessageRouting mr;
    if (useCellNetwork()) {
        mr = getOs().getCellModule().createMessageRouting()
                .destination("server_0")
                .topological()
                .build();
    } else {
        mr = getOs().getAdHocModule().createMessageRouting()
                .broadcast()
                .geographical(dest)
                .build();
    }
    
    Denm denm = new Denm(mr, new DENMContent(
        getOs().getSimulationTime(), vehiclePosition, roadId, type, strength, SPEED, 0.0f, vehiclePosition, null, null)
    );
    
    if (useCellNetwork()) {
        getOs().getCellModule().sendV2XMessage(denm);
    } else {
        getOs().getAdHocModule().sendV2XMessage(denm);
    }
}
```

Next, we look at the receiving side of the DENM here. Since the message is a simple V2X message it
is received through the `receiveV2xMessage()`-method which is part of the application interface. 
Analogous to a normal message, here we check with `instanceof`, if it is of a type that we are interested in, in
this case `Denm`. In that very case, we perform a potential route change if not already done.


```java
public void receiveV2xMessage(ReceivedV2xMessage receivedV2xMessage) {
    final V2xMessage msg = receivedV2xMessage.getMessage();
    if (!(msg instanceof Denm)) {
        return;
    }
    
    final Denm denm = (Denm)msg;
    if (routeChanged) {
        getLog().infoSimTime(this, "Route already changed");
    } else {
        reactUponDENMessageChangeRoute(denm);
    }
}
```

In order to calculate alternative routes to the destination and to change the actual route to the next best route, 
the method `circumnavigateAffectedRoad()` is implemented. The affected road segment is identified by the road id parameter.
The route calculation needs to be parametrized in a way, that it avoids this road segment to include in the calculation.
This is achieved by using the specific cost function `ReRouteSpecificConnectionsCostFunction` and passing it
to the `RoutingParameters` object.

```java
private void circumnavigateAffectedRoad(DENM denm, final String affectedRoadId) {

    ReRouteSpecificConnectionsCostFunction myCostFunction = new ReRouteSpecificConnectionsCostFunction();
    myCostFunction.setConnectionSpeedMS(affectedRoadId, denm.getCausedSpeed());
    
    NavigationModule navigationModule = getOs().getNavigationModule();
    
    RoutingParameters routingParameters = new RoutingParameters().costFunction(myCostFunction);
    
    RoutingResponse response = navigationModule.calculateRoutes(new RoutingPosition(navigationModule.getTargetPosition()), routingParameters);
    
    CandidateRoute newRoute = response.getBestRoute();
    if (newRoute != null) {
        getLog().infoSimTime(this, "Sending Change Route Command at position: {}", denm.getSenderPosition());
        navigationModule.switchRoute(newRoute);
    }
}
```

## WeatherWarningAppCell

The [`WeatherWarningAppCell`](https://github.com/eclipse-mosaic/mosaic/blob/main/app/tutorials/weather-warning/src/main/java/org/eclipse/mosaic/app/tutorial/WeatherWarningAppCell.java) extends the previously presented `WeatherWarningApp` by using communication
via a cellular network instead of ad-hoc based communication. In that case, DENM are not 
sent to vehicles directly, but to the **WeatherServer** (addressed by name `server_0`), which then will 
re-broadcast received DENM to all cell-connected vehicles in the near of the hazardous area.   

## WeatherServerApp

The [`WeatherServerApp`](https://github.com/eclipse-mosaic/mosaic/blob/main/app/tutorials/weather-warning/src/main/java/org/eclipse/mosaic/app/tutorial/WeatherServerApp.java) is mapped to a server simulation unit without geographical location which receives knowledge about the hazardous area via DEN-messages
and it is responsible to re-transmit those DEN-Messages periodically to vehicles equipped with `WeatherWarningAppCell`-application via 
cellular communication. Firstly, the interval for re-transmitting the last received DEN-message will be defined:

```java
 /**
 * Send warning at this interval, in seconds.
 */
private final static long INTERVAL = 2 * TIME.SECOND;


/**
 * Save the last received DEN message for relaying.
 */
private Denm lastReceivedDenm = null;
```

During the initialization procedure of communicating applications, the communication module (CellModule) needs to be
activated. This is achieved in the `onStartup()`-method. The following code snippet shows the activating the
CellModule as communication mode:

```java
public void onStartup() {
    getLog().infoSimTime(this, "Initialize WeatherServer application");
    getOperatingSystem().getCellModule().enable();
    sample();
}
```

In order to re-transmit the last received DENM, we need to capture all received V2X messages and store the last received DENM.
This is achieved by implementing the `onMessageReceived` method. 

```java
@Override
public void onMessageReceived(ReceivedV2xMessage receivedV2xMessage) {
    final V2xMessage msg = receivedV2xMessage.getMessage();
    
    // Only DEN Messages are handled
    if (msg instanceof Denm) {
        lastReceivedDenm = (Denm) msg;
    }
}
```

The last received DENM will then be re-transmitted in **2s** intervals (as configured above) to the vehicles equipped with cellular
communication within reach as an event;

```java
private void sample() {
    if (lastReceivedDenm != null) {        
        final Denm denm = constructDenm();
        getOs().getCellModule().sendV2xMessage(denm);
    }
    getOs().getEventManager().addEvent(getOs().getSimulationTime() + INTERVAL, this);
}
```

The `Event` created in this method is passed with a `this` argument, which represents an `EventProcessor` that gets notified
as soon as the proposed event time is reached. As this class implements the `EventProcessor` interface, the method
`processEvent()` will be called by the application simulator once the simulation time is reached:

```java
@Override
public void processEvent(Event event) throws Exception {
    sample();
}
```

Finally, in the method `contructDenm`, a new DENM object will be created for transmitting, by copying
the `DenmContent` of the last received DENM. Furthermore, we create a circular area for the rerouting (3000 m) 
of the DEN-Message as Broadcast. All vehicles within this area which have the cellular module activated, will 
receive this message.

```java
private Denm constructDenm() {
    final MessageRouting routing = getOs().getCellModule().createMessageRouting()
            .broadcast()
            .geographical(new GeoCircle(lastReceivedDenm.getEventLocation(), 3000.0))
            .build();
    return new Denm(routing,
            new DenmContent(
                    lastReceivedDenm.getTime(),
                    lastReceivedDenm.getSenderPosition(),
                    lastReceivedDenm.getEventRoadId(),
                    lastReceivedDenm.getWarningType(),
                    lastReceivedDenm.getEventStrength(),
                    lastReceivedDenm.getCausedSpeed(),
                    lastReceivedDenm.getSenderDeceleration(),
                    lastReceivedDenm.getEventLocation(),
                    lastReceivedDenm.getEventArea(),
                    lastReceivedDenm.getExtendedContainer()
            ),
            200
    );
}
```

