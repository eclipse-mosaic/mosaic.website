---
title: Environment Configuration
linktitle: Environment
toc: true
type: docs
draft: false
weight: 102
menu:
  docs:
    parent: mosaic_configuration
---

{{% alert tip %}}
You can use a {{< link title="Readable Unit" href="/docs/mosaic_configuration/#readable-units">}} anywhere where you
have the option of:
- `number` **or** `string`
- `integer` **or** `string`
{{% /alert %}}

---

<span class="page_with_curl">:page_with_curl:</span> **Corresponding configuration file: `environment/environment_config.json`**



---------------------------------------
<a name="reference-environment-events"></a>
## Environment Events

Schema describing the JSON file structure for the environment configuration. It is is used to define which events are evaluated by the simulator. Entities entering the areas of the events, are notified by the 'EnvironmentSensorUpdates' interaction

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|events|[`event[]`](#reference-event)|List of events.|No|None|None|



---------------------------------------
<a name="reference-event"></a>
## event

Object to define a single event configuration.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|location|[`eventLocation`](#reference-eventlocation)|Object to define the event location configuration, which is either a GeoArea (e.g. rectangle, circle, or polygon) or a specific street segment.|No|None|None|
|time|[`eventTime`](#reference-eventtime)|Object to define the temporal properties of an event.|No|None|None|
|type|`string`|Represents the type of event value this event is emitting (e.g., TRACTION_HAZARD, VISIBILITY_HAZARD, WIND, TEMPERATURE, or an arbitrary EVENT). The value can be set to any valid string.| &#10003; Yes|None|None|
|value|`string`<br>`number`<br>`object`|Used for assigning a value to the event. It can be used as the strength of an event, the road surface quality, or the name of a specific event cause, etc.| &#10003; Yes|None|None|



---------------------------------------
<a name="reference-eventlocation"></a>
## eventLocation

Object to define the event location configuration, which is either a GeoArea (e.g. rectangle, circle, or polygon) or a specific street segment.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|area|[`area`](#reference-area)|Object to define the area in which the event is located in.|No|None|None|
|connectionId|`string`|The ID of the road segment the event is located on (Connection ID).|No|None|None|



---------------------------------------
<a name="reference-area"></a>
## area

Object to define the area in which the event is located in.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|type|`string`|The type of the area.|No|Enum[<i class="fas fa-info-circle"></i>](#restriction-areatype)|None|

**Additionally ANY of the following property definitions apply:**  
* [geoCircle](#reference-geoCircle) 
* [geoRectangle](#reference-geoRectangle) 
* [geoPolygon](#reference-geoPolygon) 

**Further property restrictions:**  
<a name="restriction-areatype"></a> 
### area.type

* **Allowed values**:
   * `Rectangle`
   * `Circle`
   * `Polygon`


---------------------------------------
<a name="reference-geocircle"></a>
## geoCircle

Object to define an immutable pair of a geoPoint center position and a radius in meters.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|center|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|radius|`number`|Radius of the circle in m.| &#10003; Yes|[0, +$\infty$]|None|



---------------------------------------
<a name="reference-geopoint"></a>
## geoPoint

Object to define geographical point coordinates.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|longitude|`number`|East-west position of a point on earth.| &#10003; Yes|[-180, 180]|None|
|latitude|`number`|North-south position of a point on earth.| &#10003; Yes|[-$\infty$, 90]|None|



---------------------------------------
<a name="reference-georectangle"></a>
## geoRectangle

Object to define an immutable pair of two different GeoPoints. Together they form a rectangular area.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|a|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|
|b|[`geoPoint`](#reference-geopoint)|Object to define geographical point coordinates.| &#10003; Yes|None|None|



---------------------------------------
<a name="reference-geopolygon"></a>
## geoPolygon

Object to define a set of GeoPoints. Together the points for a polygonal area.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|vertices|[`geoPoint[]`](#reference-geopoint)|Set of points representing the vertices of a polygon.|No|None|None|



---------------------------------------
<a name="reference-eventtime"></a>
## eventTime

Object to define the temporal properties of an event.

**Properties**

|   |Type|Description|Required|Boundaries|Default|
|---|---|---|---|---|---|
|start|`string`<br>`number`|Start time of the event. If defined as a number, then the default unit is ns. Alternatively this can be defined as a string to include the unit of measurement (e.g. '3 minutes')| &#10003; Yes|None|None|
|end|`string`<br>`number`|End time of the event. If defined as a number, then the default unit is ns. Alternatively this can be defined as a string to include the unit of measurement (e.g. '3 minutes')| &#10003; Yes|None|None|

